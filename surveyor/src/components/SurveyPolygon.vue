<template>
  <svg
    ref="svgRef"
    class="overflow-visible"
    width="460"
    height="500"
    viewBox="0 0 460 500"
    aria-hidden="true"
  >
    <!-- North indicator -->
    <g :style="{ opacity: phase >= 6 ? 0.8 : 0, transition: 'opacity 0.6s' }">
      <line x1="380" y1="60" x2="380" y2="30" stroke="#c9a96e" stroke-width="1"/>
      <polygon points="380,22 376,32 380,29 384,32" fill="#c9a96e"/>
      <text x="380" y="72" text-anchor="middle" class="survey-tiny">N</text>
    </g>

    <!-- Internal dashed diagonals -->
    <g :style="{ opacity: phase >= 6 ? 1 : 0, transition: 'opacity 0.6s' }">
      <line v-for="d in diagonals" :key="d.key"
        :x1="d.x1" :y1="d.y1" :x2="d.x2" :y2="d.y2"
        fill="none" stroke="#c9a96e" stroke-width="0.6"
        stroke-dasharray="4 4" opacity="0.35"
      />
    </g>

    <!-- Polygon segments -->
    <line
      v-for="(seg, i) in segments"
      :key="'seg' + i"
      :x1="seg.x1" :y1="seg.y1"
      :x2="seg.x2" :y2="seg.y2"
      fill="none"
      stroke="#c9a96e"
      stroke-width="1.2"
      stroke-linecap="round"
      stroke-linejoin="round"
      :style="segStyle(i)"
    />

    <!-- Dimension labels -->
    <g :style="{ opacity: phase >= 6 ? 1 : 0, transition: 'opacity 0.5s' }">
      <text x="195" y="133" text-anchor="middle" class="survey-dim">N 84°07'E — 142.6m</text>
      <text x="395" y="195" text-anchor="start" class="survey-dim" transform="rotate(72,395,195)">S 12°41'E — 98.3m</text>
      <text x="330" y="355" text-anchor="middle" class="survey-dim">S 78°40'W — 117.2m</text>
      <text x="184" y="402" text-anchor="middle" class="survey-dim">W — 95.1m</text>
      <text x="62" y="298" text-anchor="end" class="survey-dim" transform="rotate(-88,62,298)">N 01°20'W — 86.4m</text>
    </g>

    <!-- Vertex markers + labels -->
    <g
      v-for="(v, i) in vertices"
      :key="'v' + i"
      :style="{ opacity: phase >= i + 1 ? 1 : 0, transition: 'opacity 0.2s' }"
    >
      <rect :x="v.x - 5" :y="v.y - 5" width="10" height="10"
        fill="none" stroke="#c9a96e" stroke-width="1.2"/>
      <text
        :x="v.labelX" :y="v.labelY"
        :text-anchor="v.anchor"
        class="survey-label"
      >{{ v.name }}</text>
    </g>

    <!-- Centre label -->
    <g :style="{ opacity: phase >= 6 ? 1 : 0, transition: 'opacity 0.8s' }">
      <text x="218" y="268" text-anchor="middle" class="parcel-name">PARCEL 17 — 0.482 HA</text>
      <text x="218" y="286" text-anchor="middle" class="parcel-sub">SURVEYED · VERIFIED · VALUED</text>
    </g>
  </svg>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Polygon vertices: PB1042 → 1043 → 1044 → 1045 → 1046 → back to 1042
const vertices = [
  { x: 80,  y: 195, name: 'PB 1042', labelX: 65,  labelY: 193, anchor: 'end'   },
  { x: 310, y: 100, name: 'PB 1043', labelX: 320, labelY: 94,  anchor: 'start' },
  { x: 370, y: 285, name: 'PB 1044', labelX: 382, labelY: 290, anchor: 'start' },
  { x: 280, y: 400, name: 'PB 1045', labelX: 292, labelY: 418, anchor: 'start' },
  { x: 90,  y: 390, name: 'PB 1046', labelX: 72,  labelY: 408, anchor: 'end'   },
]

// Build segments from consecutive vertex pairs, closing the loop
const segments = vertices.map((v, i) => {
  const next = vertices[(i + 1) % vertices.length]
  return {
    x1: v.x, y1: v.y,
    x2: next.x, y2: next.y,
    length: Math.hypot(next.x - v.x, next.y - v.y),
  }
})

// Durations per segment (ms)
const durations = [600, 500, 520, 480, 420]

const diagonals = [
  { key: 'a', x1: 80, y1: 195, x2: 310, y2: 100 },
  { key: 'b', x1: 80, y1: 195, x2: 370, y2: 285 },
  { key: 'c', x1: 80, y1: 195, x2: 280, y2: 400 },
  { key: 'd', x1: 310, y1: 100, x2: 280, y2: 400 },
  { key: 'e', x1: 370, y1: 285, x2: 160, y2: 65  },
]

// phase: 0 = nothing, 1 = dot0 visible, 2 = seg0 drawn + dot1, …, 6 = all details
const phase = ref(0)
// per-segment draw progress 0→1
const progress = ref(segments.map(() => 0))

function segStyle(i) {
  const len = segments[i].length
  const p = progress.value[i]
  const offset = len * (1 - p)
  return {
    strokeDasharray: len,
    strokeDashoffset: offset,
    transition: p > 0 && p < 1
      ? `stroke-dashoffset ${durations[i]}ms ease-in-out`
      : 'none',
  }
}

onMounted(() => {
  // Show first dot
  setTimeout(() => { phase.value = 1 }, 300)

  let delay = 400
  segments.forEach((seg, i) => {
    // Trigger line draw
    setTimeout(() => {
      progress.value = progress.value.map((v, j) => j === i ? 0 : v)
      // Force a tiny delay so the dashoffset=len is rendered before we transition
      requestAnimationFrame(() => {
        requestAnimationFrame(() => {
          progress.value = progress.value.map((v, j) => j === i ? 1 : v)
        })
      })
    }, delay)

    // Show next vertex dot when segment finishes
    setTimeout(() => {
      phase.value = i + 2   // dot indices: 1=PB1042, 2=PB1043, …
    }, delay + durations[i])

    delay += durations[i] + 60
  })

  // Reveal internal details after all segments
  setTimeout(() => { phase.value = 6 }, delay + 200)
})
</script>

<style scoped>
.survey-label {
  fill: #c9a96e;
  font-family: 'Inter', sans-serif;
  font-size: 9px;
  letter-spacing: 0.05em;
}
.survey-dim {
  fill: #c9a96e;
  font-family: 'Inter', sans-serif;
  font-size: 8.5px;
  letter-spacing: 0.04em;
}
.survey-tiny {
  fill: #c9a96e;
  font-family: 'Inter', sans-serif;
  font-size: 9px;
  letter-spacing: 0.1em;
}
.parcel-name {
  fill: #c9a96e;
  font-family: 'Cormorant Garamond', serif;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.12em;
}
.parcel-sub {
  fill: #c9a96e;
  font-family: 'Inter', sans-serif;
  font-size: 8px;
  letter-spacing: 0.18em;
  opacity: 0.7;
}
</style>