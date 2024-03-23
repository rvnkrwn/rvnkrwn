<template>
  <div>
    <WaterFluid />
    <section id="hero">
      <div id="hero-content">
        <h1 class="flex justify-between">
          <span>R</span>
          <span>E</span>
          <span>V</span>
          <span>A</span>
          <span>N</span>
        </h1>
        <h2 class="tracking-wide">Kurniawan</h2>
        <p class="text-center text-lg sm:text-2xl font-thin opacity-70 mt-4">ꦫꦺꦮ꦳ꦟ꧀ꦑꦸꦂꦟꦶꦄꦮꦟ꧀</p>
      </div>
    </section>

    <section id="content" class="bg-uranian-blue !hidden">

    </section>
  </div>
</template>

<script setup lang="ts">

import WaterFluid from "~/components/three_components/water_fluid/WaterFluid.vue";
</script>

<style scoped>
#hero {
  @apply flex w-full h-[100dvh] items-center justify-center select-none
}

#hero-content {
  @apply text-center uppercase font-poppins text-white font-black text-4xl sm:text-6xl md:text-7xl
}

#content {
  @apply h-[200dvh] w-full
}
</style>
