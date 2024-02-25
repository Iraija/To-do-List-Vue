<template>
  <div class="flex flex-col min-h-screen bg-green-200">
    <Navigation />
    <Contents />
  </div>
</template>

<script setup>
  import Contents from './components/Contents.vue';
  import Navigation from './components/Navigation.vue';
</script>