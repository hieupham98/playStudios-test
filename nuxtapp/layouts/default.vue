<script setup>
import Menu from './components/Header/Menu.vue'

</script>

<template>
      <NuxtLoadingIndicator />

    <header class="fixed top-0 w-[100%]">
    <!-- <img alt="Vue logo" class="logo" src="./assets/play-logo-footer.png"  height="50" /> -->
    <Menu />
    <!-- <div class="wrapper">
      <HelloWorld msg="You did it!" />
      <el-button type="primary">Primary</el-button>

    </div> -->
  </header>

  <main>
    <div class="container m-auto mt-20">
      <slot />
    </div>
    
  </main>
</template>
