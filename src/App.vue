<template>
  <div class="bg-black min-h-screen">
    <div class="container mx-auto p-4">
      <div class="flex justify-center mb-8">
        <img src="/src/assets/SpaceX.svg" alt="SpaceX Logo" class="w-64 h-10" />
      </div>
      <NextLaunch />
    </div>
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