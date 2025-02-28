<template>
    <section class="next-launch p-4 bg-gray-100 rounded-lg mb-8">
      <h2 class="text-2xl font-bold mb-2">Prochain lancement</h2>
      <div v-if="nextLaunch">
        <p class="text-xl">{{ nextLaunch.name }}</p>
        <p>{{ formattedDate }}</p>
        <p class="text-lg">{{ countdown }}</p>
      </div>
    </section>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, computed, onMounted } from 'vue';
  import axios from 'axios';
  
  export default defineComponent({
    name: 'NextLaunch',
    setup() {
      const nextLaunch = ref<any>(null);
      const countdown = ref('00:00:00');
  
      const fetchNextLaunch = async () => {
        try {
          const { data } = await axios.get('https://api.spacexdata.com/v5/launches/next');
          nextLaunch.value = data;
          updateCountdown();
        } catch (error) {
          console.error("Erreur lors de la récupération du prochain lancement :", error);
        }
      };
  
      const formattedDate = computed(() => {
        if (!nextLaunch.value) return '';
        const date = new Date(nextLaunch.value.date_utc);
        return date.toLocaleDateString();
      });
  
      const updateCountdown = () => {
        if (!nextLaunch.value) return;
        const launchTime = new Date(nextLaunch.value.date_utc).getTime();
        const now = Date.now();
        const diff = launchTime - now;
        if (diff <= 0) {
          countdown.value = "Lancé";
          return;
        }
        const seconds = Math.floor(diff / 1000) % 60;
        const minutes = Math.floor(diff / (1000 * 60)) % 60;
        const hours = Math.floor(diff / (1000 * 60 * 60)) % 24;
        const days = Math.floor(diff / (1000 * 60 * 60 * 24));
        countdown.value = `${days}j ${hours}h ${minutes}m ${seconds}s`;
      };
  
      onMounted(() => {
        fetchNextLaunch();
        setInterval(updateCountdown, 1000);
      });
  
      return {
        nextLaunch,
        formattedDate,
        countdown,
      };
    }
  });
  </script>
  