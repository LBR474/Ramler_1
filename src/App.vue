<script setup lang="ts">
import Ramler from './components/Ramler.vue'
//import Ramler_2 from './components/Ramler_2.vue'


</script>

<template>
 
  <Suspense>
  <Ramler />
  </Suspense>
  
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
