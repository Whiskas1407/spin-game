<script setup>
import { ref, onMounted, nextTick, defineExpose, watch } from 'vue'
import lottie from 'lottie-web'
import pako from 'pako'

const props = defineProps({
  count: { type: Number, default: 1 } // 1, 3 или 5
})

const stageRef = ref(null)
const layers = ref({ bg: null, main: null }) // bg и main остаются одни
let spinning = false
let spinTimer = null

const PATH = '/roulette-file'
const files = {
  main: `${PATH}/main.tgs`,
  block: `${PATH}/block.tgs`,
  win: `${PATH}/win.tgs`,
  spins: { left: `${PATH}/spin-left.tgs`, center: `${PATH}/spin-center.tgs`, right: `${PATH}/spin-right.tgs` },
  results: {
    left: { bar: `${PATH}/left-bar.tgs`, grape: `${PATH}/left-grape.tgs`, lemon: `${PATH}/left-lemon.tgs`, seven: `${PATH}/left-7.tgs`, sevenWin: `${PATH}/left-7-win.tgs` },
    center: { bar: `${PATH}/center-bar.tgs`, grape: `${PATH}/center-grape.tgs`, lemon: `${PATH}/center-lemon.tgs`, seven: `${PATH}/center-7.tgs`, sevenWin: `${PATH}/center-7-win.tgs` },
    right: { bar: `${PATH}/right-bar.tgs`, grape: `${PATH}/right-grape.tgs`, lemon: `${PATH}/right-lemon.tgs`, seven: `${PATH}/right-7.tgs`, sevenWin: `${PATH}/right-7-win.tgs` }
  }
}

// --- UTILS ---
async function loadTgs(url) {
  const res = await fetch(url)
  if (!res.ok) throw new Error(`HTTP ${res.status} for ${url}`)
  const arrayBuffer = await res.arrayBuffer()
  const uint8 = new Uint8Array(arrayBuffer)
  const isGzip = uint8[0] === 0x1f && uint8[1] === 0x8b
  if (isGzip) return JSON.parse(pako.ungzip(uint8, { to: 'string' }))
  return JSON.parse(new TextDecoder().decode(uint8))
}

function createAnim({ container, data, loop = true, autoplay = true }) {
  const anim = lottie.loadAnimation({
    container,
    loop,
    autoplay,
    animationData: data,
    renderer: 'canvas',
    rendererSettings: { preserveAspectRatio: 'xMidYMid meet', clearCanvas: true, progressiveLoad: true }
  })
  const canvas = container.querySelector('canvas')
  if (canvas) { canvas.style.width = '100%'; canvas.style.height = '100%'; canvas.style.display = 'block' }
  return anim
}

async function swapAnim(layerId, url, opts = {}) {
  layers.value[layerId]?.destroy()
  layers.value[layerId] = null
  const container = document.getElementById(layerId)
  if (!container) return
  const data = await loadTgs(url)
  layers.value[layerId] = createAnim({ container, data, loop: opts.loop ?? false, autoplay: opts.autoplay ?? true })
  return layers.value[layerId]
}

// --- Helpers ---
function pick(arr) { return arr[Math.floor(Math.random() * arr.length)] }
function randomInt(a, b) { return Math.floor(a + Math.random() * (b - a)) }

// --- Layout и подготовка рулеток ---
async function updateLayout(count) {
  if (!stageRef.value) return
  const stage = stageRef.value
  // ширина и высота stage
  if (count === 1) { stage.style.width = '30rem'; stage.style.height = '27.5rem' }
  else if (count === 3) { stage.style.width = '18rem'; stage.style.height = '17rem' }
  else if (count === 5) { stage.style.width = '14rem'; stage.style.height = '14rem' }

  // удаляем лишние рулетки (кроме reelL/C/R)
  const reels = Array.from(stage.querySelectorAll('[id^=reel]'))
  reels.forEach(el => { if (!['reelL','reelC','reelR'].includes(el.id)) el.remove() })

  // добавляем дополнительные рулетки, если count > 3
  for (let i = 3; i < count; i++) {
    const div = document.createElement('div')
    div.id = `reel${i}`
    div.className = 'layer'
    stage.appendChild(div)
    layers.value[div.id] = null
  }

  await nextTick() // ждем пока DOM обновится
}

// --- Spin ---
async function startSpin() {
  if (spinning) return
  spinning = true

  layers.value.main?.goToAndStop(0, true)
  layers.value.main?.play()
  await swapAnim('bg', files.block)

  const allReels = Array.from(stageRef.value.querySelectorAll('[id^=reel]'))

  await Promise.all(allReels.map((r, i) => {
    const spinFile = i === 0 ? files.spins.left : i === 1 ? files.spins.center : i === 2 ? files.spins.right : files.spins.center
    return swapAnim(r.id, spinFile, { loop: true })
  }))

  spinTimer = setTimeout(settleRandomOutcome, randomInt(1600, 2600))
}

async function settleRandomOutcome() {
  const symbols = ['bar', 'grape', 'lemon', 'seven']
  const allReels = Array.from(stageRef.value.querySelectorAll('[id^=reel]'))
  const outcome = allReels.map(() => pick(symbols))
  const is777 = outcome.every(s => s === 'seven')

  await Promise.all(allReels.map((r, i) => {
    const pos = i === 0 ? 'left' : i === 1 ? 'center' : i === 2 ? 'right' : 'center'
    const symbol = outcome[i]
    const file = is777 && symbol === 'seven' ? files.results[pos].sevenWin : files.results[pos][symbol]
    return swapAnim(r.id, file)
  }))

  if (is777) setTimeout(() => swapAnim('bg', files.win, { loop: false, autoplay: true }), 1400)
  spinning = false
}

async function forceStop() {
  if (!spinning) return
  clearTimeout(spinTimer)
  await settleRandomOutcome()
}

// --- Mounted ---
onMounted(async () => {
  await swapAnim('bg', files.block, { loop: true, autoplay: true })
  await swapAnim('main', files.main, { loop: false, autoplay: false })
  updateLayout(props.count)
})

// --- Watch ---
watch(() => props.count, (newCount) => { updateLayout(newCount) })

defineExpose({ startSpin, forceStop })
</script>

<template>
  <div>
    <div class="stage" ref="stageRef">
      <div id="bg" class="layer" aria-label="background"></div>

      <div id="reelL" class="layer" aria-label="left reel"></div>
      <div id="reelC" class="layer" aria-label="center reel"></div>
      <div id="reelR" class="layer" aria-label="right reel"></div>

      <div id="main" class="layer" aria-label="top overlay"></div>
    </div>
  </div>
</template>

<style scoped>
.stage {
  position: relative;
  width: 30rem;
  height: 27.5rem;
  margin: auto;
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
