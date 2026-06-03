<template>
  <div class="min-h-screen bg-white text-gray-900 font-sans selection:bg-emerald-800 selection:text-white scroll-smooth">
    
    <header class="sticky top-0 z-50 bg-white/95 backdrop-blur border-b border-gray-100">
      <div class="max-w-7xl mx-auto px-6 h-20 flex items-center justify-between">
        <a href="#" class="text-2xl font-bold tracking-tight text-gray-900">Propsensgroup</a>
        <nav class="hidden md:flex items-center gap-8 text-xs font-semibold tracking-widest text-gray-500 uppercase">
          <a href="#home" class="text-emerald-700">Home</a>
          <a href="#about" class="hover:text-gray-900 transition">About</a>
          <a href="#services" class="hover:text-gray-900 transition">Services</a>
          
          <a href="#contact" class="hover:text-gray-900 transition">Contact</a>
        </nav>
        <a href="#appointment" class="border border-gray-900 px-5 py-2.5 text-xs font-semibold tracking-wider hover:bg-gray-900 hover:text-white transition">Get Started</a>
      </div>
    </header>

    <main>
      <router-view />
    </main>

    <footer class="bg-[#0a2e1d] text-white pt-16 pb-12">
      <div class="max-w-7xl mx-auto px-6 grid grid-cols-1 md:grid-cols-4 gap-12 text-xs font-medium text-emerald-100/60">
        <div>
          <h2 class="text-2xl font-bold text-white tracking-tight mb-4">Propsensgroup</h2>
          <p>© 2026 Propsensgroup. All rights reserved.</p>
        </div>
        <div>
          <h5 class="text-white uppercase font-bold tracking-wider mb-4">Exploration Links</h5>
          <ul class="space-y-2">
            <li><router-link to="/" class="hover:text-white">Home Landing</router-link></li>
            <li><router-link to="/additional" class="hover:text-white">Additional Portal</router-link></li>
          </ul>
        </div>
        <div>
          <h5 class="text-white uppercase font-bold tracking-wider mb-4">Support Info</h5>
          <p>Privacy Strategy Documents</p>
        </div>
        <div>
          <h5 class="text-white uppercase font-bold tracking-wider mb-4">Contact</h5>
          <p>hello@reallygreatsite.com</p>
        </div>
      </div>
    </footer>

    <footer class="bg-neutral-950 text-neutral-500 text-center py-6 text-[11px] font-medium tracking-wide">
      &copy; 2026 Propsensgroup. All rights reserved. 
    </footer>

  </div>
</template>