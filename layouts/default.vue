<template>
  <div>
    <AppHeader />
    <slot />
    <AppFooter />
  </div>
</template>

<script setup lang="ts">
import AppHeader from "~/components/layouts/app/AppHeader.vue";
import AppFooter from "~/components/layouts/app/AppFooter.vue";
</script>
