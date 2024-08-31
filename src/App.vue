<script setup>
import SplashScreen from './components/SplashScreen.vue';
import { defineAsyncComponent, ref } from 'vue';
const showSplash = ref(true); 
const HomePage = defineAsyncComponent(() => {
  return import('./components/HomePage.vue');
});
setTimeout(() => {
  showSplash.value = false; // Hide splash screen after 5 seconds
}, 2000); 
</script>

<template>
  <SplashScreen v-if="showSplash" />
  <HomePage v-else /> 
</template>

<style>
  html,
  body,
  .app {
    min-height: 100vh;
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
  }
  * {
    --brand-green: #04b500;
    --brand-blue: #0689b0;
  }
</style>
