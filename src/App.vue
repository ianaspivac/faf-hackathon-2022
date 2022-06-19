<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <RouterView />
</template>

<style>
@import '@/assets/base.css';

body {
  background-color: #111119;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
  color: #00FF93;
}
.item {
   border: 2px solid #00ff93;
   padding: 10px;
}
a {
  text-decoration: none;
}
</style>
