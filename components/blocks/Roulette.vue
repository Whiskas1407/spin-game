<script setup lang="ts">
import { ref, onMounted, defineExpose } from 'vue';
import lottie from 'lottie-web';
import pako from 'pako';

const props = defineProps({
  uid: { type: String, required: true } // уникальный ID для рулетки
});

const stageRef = ref<HTMLElement | null>(null);
const layers = ref<{ [key: string]: any }>({ bg: null, main: null });
let spinning = false;
let spinTimer: ReturnType<typeof setTimeout> | null = null;

const PATH = '/roulette-file';
const files = {
  main: `${PATH}/main.tgs`,
  block: `${PATH}/block.tgs`,
  win: `${PATH}/win.tgs`,
  spins: {
    left: `${PATH}/spin-left.tgs`,
    center: `${PATH}/spin-center.tgs`,
    right: `${PATH}/spin-right.tgs`
  },
  results: {
    left: { bar: `${PATH}/left-bar.tgs`, grape: `${PATH}/left-grape.tgs`, lemon: `${PATH}/left-lemon.tgs`, seven: `${PATH}/left-7.tgs`, sevenWin: `${PATH}/left-7-win.tgs` },
    center: { bar: `${PATH}/center-bar.tgs`, grape: `${PATH}/center-grape.tgs`, lemon: `${PATH}/center-lemon.tgs`, seven: `${PATH}/center-7.tgs`, sevenWin: `${PATH}/center-7-win.tgs` },
    right: { bar: `${PATH}/right-bar.tgs`, grape: `${PATH}/right-grape.tgs`, lemon: `${PATH}/right-lemon.tgs`, seven: `${PATH}/right-7.tgs`, sevenWin: `${PATH}/right-7-win.tgs` }
  }
};

async function loadTgs(url: string) {
  const res = await fetch(url);
  const buf = await res.arrayBuffer();
  const uint8 = new Uint8Array(buf);
  const isGzip = uint8[0] === 0x1f && uint8[1] === 0x8b;
  return JSON.parse(isGzip ? pako.ungzip(uint8, { to: 'string' }) : new TextDecoder().decode(uint8));
}

function createAnim({ container, data, loop = true, autoplay = true }: { container: HTMLElement, data: any, loop?: boolean, autoplay?: boolean }) {
  return lottie.loadAnimation({
    container,
    loop,
    autoplay,
    animationData: data,
    renderer: 'canvas',
    rendererSettings: { preserveAspectRatio: 'xMidYMid meet', clearCanvas: true }
  });
}

async function swapAnim(layerId: string, url: string, opts: { loop?: boolean; autoplay?: boolean } = {}) {
  layers.value[layerId]?.destroy();
  layers.value[layerId] = null;
  const container = document.getElementById(`${props.uid}-${layerId}`);
  if (!container) return;
  const data = await loadTgs(url);
  layers.value[layerId] = createAnim({ container, data, loop: opts.loop ?? false, autoplay: opts.autoplay ?? true });
  return layers.value[layerId];
}

function pick<T>(arr: T[]) {
  return arr[Math.floor(Math.random() * arr.length)];
}

async function startSpin() {
  if (spinning) return;
  spinning = true;

  layers.value.main?.goToAndStop(0, true);
  layers.value.main?.play();
  await swapAnim('bg', files.block);

  const reels = ['reelL', 'reelC', 'reelR'];
  await Promise.all(reels.map((r, i) => {
    const spinFile = i === 0 ? files.spins.left : i === 1 ? files.spins.center : files.spins.right;
    return swapAnim(r, spinFile, { loop: true });
  }));

  spinTimer = setTimeout(settleRandomOutcome, Math.floor(1600 + Math.random() * 1000));
}

async function settleRandomOutcome() {
  const symbols = ['bar', 'grape', 'lemon', 'seven'];
  const outcome = ['left', 'center', 'right'].map(() => pick(symbols));
  const is777 = outcome.every(s => s === 'seven');

  await Promise.all(['left', 'center', 'right'].map((pos, i) => {
    const r = ['reelL', 'reelC', 'reelR'][i];
    const symbol = outcome[i];
    const file = is777 && symbol === 'seven' ? files.results[pos].sevenWin : files.results[pos][symbol];
    return swapAnim(r, file);
  }));

  if (is777) setTimeout(() => swapAnim('bg', files.win, { loop: false, autoplay: true }), 1400);
  spinning = false;
}

async function forceStop() {
  if (!spinning) return;
  if (spinTimer) clearTimeout(spinTimer);
  await settleRandomOutcome();
}

onMounted(async () => {
  await swapAnim('bg', files.block, { loop: true, autoplay: true });
  await swapAnim('main', files.main, { loop: false, autoplay: false });
});

defineExpose({ startSpin, forceStop });
</script>

<template>
  <div class="stage" ref="stageRef">
    <div :id="`${uid}-bg`" class="layer"></div>
    <div :id="`${uid}-reelL`" class="layer"></div>
    <div :id="`${uid}-reelC`" class="layer"></div>
    <div :id="`${uid}-reelR`" class="layer"></div>
    <div :id="`${uid}-main`" class="layer"></div>
  </div>
</template>

<style scoped>
.stage {
  position: relative;
  width: 30rem;
  height: 27.5rem;
  touch-action: manipulation;
}

.layer {
  position: absolute;
  inset: 0;
}

#bg { z-index: 0; display: flex; }
#reelL, #reelC, #reelR { z-index: 1; }
#main { z-index: 2; pointer-events: none; display: flex; }

.controls {
  display: flex;
  gap: .8rem;
  justify-content: center;
  flex-wrap: wrap;
  margin: .8rem auto 2.4rem;
}

button {
  padding: .8rem 1.2rem;
  border: 0;
  border-radius: 1rem;
  background: #1f6feb;
  color: #fff;
}

button:active { transform: scale(0.98); }
</style>