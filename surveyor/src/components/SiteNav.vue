<template>
  <nav class="bg-stone-50 border-b border-stone-200 sticky top-0 z-50">
    <div class="max-w-screen-xl mx-auto px-10 h-[60px] flex items-center justify-between">

      <a href="#" class="flex items-center gap-2.5 no-underline">
        <svg width="28" height="28" viewBox="0 0 28 28" fill="none">
          <rect x="2" y="2" width="24" height="24" rx="1" stroke="#132a1e" stroke-width="1.2"/>
          <line x1="8" y1="2" x2="8" y2="26" stroke="#c9a96e" stroke-width="0.8"/>
          <line x1="20" y1="2" x2="20" y2="26" stroke="#c9a96e" stroke-width="0.8"/>
          <line x1="2" y1="8" x2="26" y2="8" stroke="#c9a96e" stroke-width="0.8"/>
          <line x1="2" y1="20" x2="26" y2="20" stroke="#c9a96e" stroke-width="0.8"/>
        </svg>
        <span class="font-sans text-[12px] font-semibold tracking-[0.15em] uppercase text-forest-900">
          PROPSENSE <span class="text-gold-400">CONSULTING</span>
        </span>
      </a>

      <div class="hidden md:flex items-center gap-8">
        <a
          v-for="link in links"
          :key="link"
          href="#"
          class="font-sans text-[13px] font-normal tracking-wide text-forest-900 no-underline transition-colors duration-200 hover:text-gold-400"
          :class="{ 'border-b border-forest-900': link === 'Home' }"
        >{{ link }}</a>
      </div>

      <a href="#" class="btn-instruct">Instruct Us</a>
    </div>
  </nav>
</template>

<script setup>
const links = ['Home', 'Firm', 'Services', 'Sectors', 'Process', 'Team', 'Contact']
</script>

<style scoped>
.btn-instruct {
  @apply inline-flex items-center bg-forest-900 text-white no-underline px-5 py-2.5
         font-sans text-[11px] font-medium tracking-[0.1em] uppercase
         transition-colors duration-200 hover:bg-forest-800 cursor-pointer;
}
</style>