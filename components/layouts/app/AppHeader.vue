<template>
  <div>
    <header>
      <div id="header-logo">
        <NuxtLink to="/">
          <img :src="logoLight" alt="" class="max-h-full">
        </NuxtLink>
      </div>
      <div id="menu-open" class="p-2 cursor-pointer" @click="handleToggleMenu">
        <Icon style="color: white" height="28" icon="iconamoon:menu-burger-horizontal-fill"
              width="28"/>
      </div>
      <div id="contact" class="mb-8">
        <ul class="flex gap-3 text-sm text-white">
          <li><NuxtLink to="" target="_blank">CONTACT</NuxtLink></li>
          <li><span class="opacity-40">/</span></li>
          <li><NuxtLink to="" target="_blank">IG</NuxtLink></li>
          <li><span class="opacity-40">/</span></li>
          <li><NuxtLink to="" target="_blank">LN</NuxtLink></li>
        </ul>
      </div>
      <div id="label-scroll" class="text-white ">
        <span>scroll</span>
        <hr class="w-20" />
      </div>
      <div id="copyright" class="text-white">
        <span>&copy; rvnkrwn Inc.</span>
      </div>
    </header>
    <nav id="menus" class="hidden">
      <div id="menu-close" class="p-2 cursor-pointer absolute top-4 right-4 z-10" @click="handleToggleMenu">
        <Icon icon="guidance:right-arrow" width="40" height="40"  style="color: white" />
      </div>
      <ul id="menus-content" class="container mx-auto px-8 py-10">
        <li class="mb-8">
          <ul class="flex gap-4">
            <li><NuxtLink to="/" target="_blank">CONTACT</NuxtLink></li>
            <li><span class="opacity-40">/</span></li>
            <li><NuxtLink to="/" target="_blank">IG</NuxtLink></li>
            <li><span class="opacity-40">/</span></li>
            <li><NuxtLink to="/" target="_blank">LN</NuxtLink></li>
          </ul>
        </li>
        <li><span class="opacity-40 text-sm">All Menus</span></li>
        <li class="menu-item">
          <div @click="handleNavigate('/')" class="menu-link">Home</div>
        </li>
        <li><hr class="opacity-40 text-sm w-7" /></li>
        <li class="menu-item">
          <div @click="handleNavigate('/about')" class="menu-link">About Me</div>
        </li>
        <li><hr class="opacity-40 text-sm w-7" /></li>
        <li class="menu-item">
          <div @click="handleNavigate('/')" class="menu-link">Skill and Experience</div>
        </li>
        <li><hr class="opacity-40 text-sm w-7" /></li>
        <li class="menu-item">
          <div @click="handleNavigate('/')" class="menu-link">Showcase</div>
        </li>
        <li><hr class="opacity-40 text-sm w-7" /></li>
        <li class="menu-item">
          <div @click="handleNavigate('/')" class="menu-link">Blog</div>
        </li>
        <li><hr class="opacity-40 text-sm w-7" /></li>
      </ul>
    </nav>
  </div>
</template>

<script lang="ts" setup>
import {Icon} from "@iconify/vue";
import {gsap} from "gsap";
import logoDark from '~/assets/img/logo/r-logo-dark.png'
import logoLight from '~/assets/img/logo/r-logo-light.png'

const isDarkMode = ref(false)

const currentTheme = computed(() => {
  return isDarkMode.value
})

const handleToggleMenu = () => {
  const tl = gsap.timeline()
  const menu = document.getElementById('menus')
  if (menu && menu.classList.contains('hidden')) {
    tl.fromTo(menu, {
      translateX: '-100%',
      onComplete: () => {
        menu.classList.remove('hidden')
      },
    }, {
      translateX: 0,
      duration: .2,
      delay: .3,
      ease: "power1.inOut"
    })

    tl.fromTo('#menus-content', {
      translateX: '-100%',
      opacity: 0
    }, {
      opacity: 1,
      translateX: 0,
      duration: .3,
      delay: .2
    })

    tl.fromTo('#menu-close', {
      opacity: 0
    }, {
      opacity: 1,
      duration: .2,
      delay: .2
    })
  } else {
    tl.to('#menus-content', {
      translateX: '-100%',
      opacity: 0,
      duration: .1,
    })

    tl.to('#menu-close', {
      opacity: 0,
      duration: .1,
    })

    tl.fromTo(menu, {
      translateX: 0,
    }, {
      translateX: '-100%',
      duration: .2,
      delay: .1,
      ease: "power1.inOut",
      onComplete: () => {
        menu?.classList.add('hidden')
      },
    })
  }
}

const handleNavigate = (location: string) => {
  handleToggleMenu()
  setTimeout(() => navigateTo(location), 500)
}

useHead({
  htmlAttrs: {
    lang: 'en',
  },
  link: [
    {rel: 'icon', href: currentTheme.value ? "favicon-light.ico" : "favicon-dark.ico"}
  ]
})
</script>

<style scoped>
#header-logo {
  @apply fixed z-[9999] h-14 top-3 left-3 md:top-4 md:left-4 md:h-16
}

#menu-open {
  @apply fixed z-[9999] right-3 top-4 md:top-1/2 md:-translate-y-1/2
}

#contact {
  @apply hidden md:block md:fixed md:z-[9999] md:right-6 md:top-8
}

#label-scroll {
  @apply hidden md:flex fixed z-[9999] bottom-24 right-0 transform translate-x-1/3 rotate-90 items-center gap-2
}

#copyright {
  @apply hidden md:block fixed z-[9999] bottom-24 left-0 transform -translate-x-1/3 rotate-90
}

#menus {
  @apply w-[100vw] h-[100dvh] bg-penn-blue/95 text-white fixed z-[99999]
}

#menus-content li {
  @apply font-poppins text-lg mt-4 md:text-xl xl:text-2xl
}

.menu-link {
  @apply cursor-pointer
}
</style>
