<template>
  <div class="mt-4 flex justify-center">
    <nav class="p-2">
      <button
        v-for="filter in filters"
        :key="filter.value"
        @click="onChangeFilter(filter.value)"
        class="mx-2 relative focus:outline-none text-white button-underline"
        :class="selectedFilter === filter.value ? 'text-blue-400 font-semibold' : 'text-gray-300 hover:text-blue-300'"
      >
        {{ filter.label }}
      </button>
    </nav>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'LaunchFilter',
  emits: ['filterChanged'],
  setup(_, { emit }) {
    const selectedFilter = ref('all');
    const filters = [
      { value: 'all', label: 'Tous les lancements' },
      { value: 'success', label: 'Lancements réussis' },
      { value: 'failure', label: 'Lancements échoués' }
    ];

    const onChangeFilter = (filter: string) => {
      selectedFilter.value = filter;
      console.log('Filtre sélectionné (LaunchFilter):', selectedFilter.value);
      emit('filterChanged', selectedFilter.value);
    };

    return { selectedFilter, filters, onChangeFilter };
  }
});
</script>

<style scoped>

.button-underline {
  padding: 0.5rem 0;
  border: none;
  background: none;
}

.button-underline::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 0;
  height: 2px;
  background-color: currentColor;
  transition: width 0.3s;
}

.button-underline:hover::after {
  width: 100%;
}
</style>
