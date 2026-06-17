<template>
  <section class="hero-bg relative overflow-hidden flex items-center">
    <!-- Grid overlay -->
    <div class="absolute inset-0 grid-overlay pointer-events-none" />

    <div class="max-w-screen-xl mx-auto px-10 w-full grid grid-cols-2 gap-16 items-center py-24 min-h-[calc(100vh-60px)]">

      <!-- Left: Text -->
      <div>
        <p class="eyebrow mb-7">Estate Surveyors &amp; Valuers · Nigeria</p>

        <h1 class="hero-heading">
          Property intelligence<br/>
          you can take to the<br/>
          <em class="text-gold-400 not-italic">bank, the board, or the<br/>court.</em>
        </h1>

        <p class="hero-body mt-7 mb-9">
          Propsense Consulting delivers independent, evidence-based property assessments
          across residential, commercial, and industrial sectors — defensible, audit-ready,
          and signed by a named professional.
        </p>

        <div class="flex gap-3 flex-wrap">
          <a href="#" class="btn-gold">Request a fee estimate</a>
          <a href="#" class="btn-ghost">Explore services</a>
        </div>

        <!-- Credentials -->
        <div class="flex flex-wrap gap-5 mt-12 pt-7 border-t border-white/[0.08]">
          <span v-for="cred in credentials" :key="cred" class="cred-item">
            <span class="cred-dot" />{{ cred }}
          </span>
        </div>
      </div>

      <!-- Right: Animated polygon -->
      <div class="flex justify-center items-center">
        <SurveyPolygon />
      </div>

    </div>
  </section>
</template>

<script setup>
import SurveyPolygon from './SurveyPolygon.vue'

const credentials = [
  'ESVARBON Registered',
  'NIESV Member Firm',
  'IVSC-Aligned',
  'Full PI Insurance',
]
</script>

<style scoped>
.hero-bg {
  background-color: #0e1f18;
}
.grid-overlay {
  background-image:
    linear-gradient(rgba(255,255,255,0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.04) 1px, transparent 1px);
  background-size: 80px 80px;
}
.eyebrow {
  @apply font-sans text-[10px] font-medium tracking-[0.15em] uppercase
         text-white/50 flex items-center gap-3.5;
}
.eyebrow::before {
  content: '';
  @apply block w-8 h-px bg-white/40;
}
.hero-heading {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(42px, 5.5vw, 68px);
  font-weight: 300;
  line-height: 1.1;
  color: #fff;
}
.hero-body {
  @apply font-sans text-sm leading-[1.8] text-white/55 max-w-[420px];
}
.btn-gold {
  @apply inline-flex items-center gap-2 border border-gold-400 text-gold-400 bg-transparent
         px-6 py-3 font-sans text-[11px] font-medium tracking-[0.12em] uppercase no-underline
         transition-colors duration-200 hover:bg-gold-400 hover:text-forest-950 cursor-pointer;
}
.btn-ghost {
  @apply inline-flex items-center gap-2 border border-white/20 text-white bg-transparent
         px-6 py-3 font-sans text-[11px] font-medium tracking-[0.12em] uppercase no-underline
         transition-colors duration-200 hover:bg-white/[0.08] cursor-pointer;
}
.cred-item {
  @apply font-sans text-[10px] font-medium tracking-[0.1em] uppercase
         text-gold-400/70 flex items-center gap-1.5;
}
.cred-dot {
  @apply w-1 h-1 rounded-full bg-gold-400 inline-block;
}
</style>