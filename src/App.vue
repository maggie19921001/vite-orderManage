<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <body>
    <nav>
    <RouterLink to="/">Home</RouterLink>
  </nav>
  <RouterView />
  </body>
</template>

<style scoped>
</style>
