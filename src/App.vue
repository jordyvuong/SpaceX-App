<template>
  <div
    class="relative min-h-screen bg-cover bg-center bg-overlay"
    style="background-image: url('/src/assets/spacex_bg.jpg')"
  >
    <div class="relative container mx-auto p-4">
      <div class="flex justify-center mb-8">
        <img src="/src/assets/SpaceX.svg" alt="SpaceX Logo" class="w-64 h-10" />
      </div>
      <NextLaunch />
      <LaunchFilter @filterChanged="updateFilter" />
      <LaunchList :filter="filter" @launchSelected="selectLaunch" />
      <LaunchModal 
        v-if="selectedLaunch"  
        :launch="selectedLaunch" 
        @close="selectedLaunch = null" 
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import NextLaunch from './components/NextLaunch.vue';
import LaunchFilter from './components/LaunchFilter.vue';
import LaunchList from './components/LaunchList.vue';
import LaunchModal from './components/LaunchModal.vue';

export default defineComponent({
  name: 'App',
  components: {
    NextLaunch,
    LaunchFilter,
    LaunchList,
    LaunchModal
  },
  setup() {
    const filter = ref('all');
    const selectedLaunch = ref(null);

    const updateFilter = (newFilter: string) => {
      filter.value = newFilter;
    };

    const selectLaunch = (launch: any) => {
      selectedLaunch.value = launch;
    };

    return { filter, updateFilter, selectLaunch, selectedLaunch };
  }
});
</script>

<style scoped>
.bg-overlay::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5); 
  z-index: 1;
}
.relative > .container {
  position: relative;
  z-index: 2;
}
</style>