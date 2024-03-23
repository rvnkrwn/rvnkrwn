<template>
  <div id="wrapper">
    <PlaneColorDynamic />
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

    <section id="content">

    </section>
  </div>
</template>

<script setup lang="ts">
import {gsap} from "gsap";
import {ScrollTrigger} from "gsap/ScrollTrigger";
import PlaneColorDynamic from "~/components/three_components/PlaneColorDynamic.vue";

onMounted(() => {
  gsap.registerPlugin(ScrollTrigger)
  const tl = gsap.timeline( {
    scrollTrigger: {
      trigger: "#hero",
      start: "top top",
      end: "+=200",
      pin: true,
      onUpdate: (self) => {
        const hero: HTMLElement | any = document.getElementById("hero");
        const content: HTMLElement | any =  document.getElementById("content");
        if (self.progress >= 0.5) {
          hero.style.display = "none";
          content.style.display = "block";
          gsap.to(window, { duration: .3, scrollTo: "#content" });
        } else {
          hero.style.display = "flex";
          content.style.display = "none";
          gsap.to(window, { duration: .3, scrollTo: "#hero" });
        }
      }
    },
    yPercent: 0,
    duration: .2
  })

})
</script>

<style scoped>
#canvas {
  position: fixed;
  height: auto;
  width: 100%;
  z-index: -1;
}

#hero {
  @apply flex w-full h-[100dvh] items-center justify-center absolute
}

#hero-content {
  @apply text-center uppercase font-poppins text-white font-black text-4xl sm:text-6xl md:text-7xl
}

#content {
  @apply h-[500dvh] w-full absolute
}
</style>
