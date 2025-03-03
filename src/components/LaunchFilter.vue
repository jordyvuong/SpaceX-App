<template>
  <div class="mt-4 flex justify-center">
    <nav class="p-2 rounded shadow">
      <button
        v-for="filter in filters"
        :key="filter.value"
        @click="onChangeFilter(filter.value)"
        :class="['px-4 py-2 mx-1 rounded', { 'bg-blue-500 text-white': selectedFilter === filter.value, 'bg-gray-200': selectedFilter !== filter.value }]"
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
button {
  transition: background-color 0.3s, color 0.3s;
}
button:hover {
  background-color: #3b82f6; /* Couleur de survol */
  color: white;
}
</style>
