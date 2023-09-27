<script setup>
import { RouterView } from 'vue-router'
import NavBar from "./components/NavBar.vue"
</script>

<template>
  <NavBar />
  <div class="max-w-screen-xl m-auto mt-8">
    <RouterView />
  </div>
</template>
