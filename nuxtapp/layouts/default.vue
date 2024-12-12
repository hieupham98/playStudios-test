<script setup>
import Menu from './components/Header/Menu.vue'

</script>

<template>
    <NuxtLoadingIndicator />
    <header class="fixed top-0 w-[100%]">
      <Menu />
    </header>

  <main>
    <div class="container m-auto mt-20">
      <slot />
    </div>
  </main>
</template>

<style>
  .header {
    z-index: 10000  !important;
  }
</style>
