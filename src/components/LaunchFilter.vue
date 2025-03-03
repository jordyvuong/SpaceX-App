<template>
  <div class="mt-4">
  <!--Remplacer le dropdown par un selecteur comme une navbar et le centrer-->
    <select
      v-model="selectedFilter"
      @change="onChangeFilter"
      class="bg-white text-black p-2 border border-gray-300 rounded"
    >
      <option value="all">Tous les lancements</option>
      <option value="success">Lancements réussis</option>
      <option value="failure">Lancements échoués</option>
    </select>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'LaunchFilter',
  emits: ['filterChanged'],
  setup(_, { emit }) {
    const selectedFilter = ref('all');

    const onChangeFilter = () => {
      console.log('Filtre sélectionné (LaunchFilter):', selectedFilter.value);
      emit('filterChanged', selectedFilter.value);
    };

    return { selectedFilter, onChangeFilter };
  }
});
</script>
