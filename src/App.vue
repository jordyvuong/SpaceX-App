<template>
  <div class="container mx-auto p-4">
    <h1 class="text-center text-4xl font-bold mb-8">SpaceX Lancements</h1>
    <NextLaunch />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import NextLaunch from './components/NextLaunch.vue';

export default defineComponent({
  name: 'App',
  components: { NextLaunch }
});
</script>