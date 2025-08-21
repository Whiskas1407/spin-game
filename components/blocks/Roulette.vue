<!--Этот компонент особо не менял! Стиля сделаны не в SCSS-->

<script setup>
import { ref, onMounted } from 'vue'
import lottie from 'lottie-web'
import pako from 'pako'

const stageRef = ref(null)

async function loadTgs(url) {
  const res = await fetch(url)
  if (!res.ok) throw new Error(`HTTP ${res.status} for ${url}`)

  const arrayBuffer = await res.arrayBuffer()
  const uint8 = new Uint8Array(arrayBuffer)

  const isGzip = uint8[0] === 0x1f && uint8[1] === 0x8b

  if (isGzip) {
    return JSON.parse(pako.ungzip(uint8, { to: 'string' }))
  } else {
    return JSON.parse(new TextDecoder().decode(uint8))
  }
}

function createAnim({ container, data, loop = true, autoplay = true, renderer = 'canvas' }) {
  return lottie.loadAnimation({
    container,
    loop,
    autoplay,
    animationData: data,
    renderer,
    rendererSettings: { progressiveLoad: true, clearCanvas: true, preserveAspectRatio: 'xMidYMid meet' }
  })
}

const layers = { bg: null, main: null, reelL: null, reelC: null, reelR: null }

const PATH = '/roulette-file'
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
}

let spinning = false
let spinTimer = null
const MIN_SPIN_MS = 1600
const MAX_SPIN_MS = 2600

async function swapAnim(layerId, url, opts = {}) {
  layers[layerId]?.destroy()
  layers[layerId] = null

  const data = await loadTgs(url)
  const container = document.getElementById(layerId)
  if (!container) return

  const anim = createAnim({
    container,
    data,
    loop: opts.loop ?? false,
    autoplay: opts.autoplay ?? true,
    renderer: 'canvas'
  })

  if ((opts.loop ?? false) === false) {
    anim.addEventListener('complete', () => {
      const lastFrame = Math.floor(anim.getDuration(true)) - 1
      if (lastFrame >= 0) anim.goToAndStop(lastFrame, true)
    })
  }

  layers[layerId] = anim
  return anim
}

async function startSpin() {
  if (spinning) return
  spinning = true

  layers.main?.goToAndStop(0, true)
  layers.main?.play()
  await swapAnim('bg', files.block)

  await Promise.all([
    swapAnim('reelL', files.spins.left, { loop: true }),
    swapAnim('reelC', files.spins.center, { loop: true }),
    swapAnim('reelR', files.spins.right, { loop: true })
  ])

  spinTimer = setTimeout(settleRandomOutcome, randomInt(MIN_SPIN_MS, MAX_SPIN_MS))
}

defineExpose({ startSpin })

async function forceStop() {
  if (!spinning) return
  clearTimeout(spinTimer)
  await settleRandomOutcome()
}

async function settleRandomOutcome() {
  const symbols = ['bar', 'grape', 'lemon', 'seven']
  const L = pick(symbols), C = pick(symbols), R = pick(symbols)
  const is777 = (L === 'seven' && C === 'seven' && R === 'seven')

  if (is777) {
    await Promise.all([
      swapAnim('reelL', files.results.left.sevenWin),
      swapAnim('reelC', files.results.center.sevenWin),
      swapAnim('reelR', files.results.right.sevenWin)
    ])
    setTimeout(() => swapAnim('bg', files.win, { loop: false, autoplay: true }), 1400)
  } else {
    await Promise.all([
      swapAnim('reelL', files.results.left[L]),
      swapAnim('reelC', files.results.center[C]),
      swapAnim('reelR', files.results.right[R])
    ])
  }

  spinning = false
}

function pick(arr) { return arr[(Math.random() * arr.length) | 0] }
function randomInt(a, b) { return (a + Math.random() * (b - a)) | 0 }

onMounted(async () => {
  await swapAnim('bg', files.block, { loop: true, autoplay: true })
  await swapAnim('main', files.main, { loop: false, autoplay: false })

  ;['reelL', 'reelC', 'reelR'].forEach(id => {
    const el = document.getElementById(id)
    if (el) el.innerHTML = ''
  })
})
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

#bg { z-index: 0; }
#reelL, #reelC, #reelR { z-index: 1; }
#main { z-index: 2; pointer-events: none; }

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
