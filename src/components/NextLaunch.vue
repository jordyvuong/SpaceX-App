<template>
    <section
      class="next-launch relative p-4 rounded-lg mb-8"
      style="background-image: url('src/assets/ussf.jpg'); background-size: cover; background-position: center;">
      <div class="absolute inset-0 bg-black opacity-30 rounded-lg"></div>
      <div class="relative z-10">
        <h2 class="text-2xl font-bold mb-2 text-white text-center">Prochain lancement</h2>
        <div v-if="nextLaunch">
          <p class="text-xl text-white text-center">{{ nextLaunch.name }}</p>
          <p class="text-white text-center">{{ formattedDate }}</p>
          <p class="text-lg text-white text-center">{{ countdown }}</p>
        </div>
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
          const { data } = await axios.get('https://api.spacexdata.com/v5/launches/next'); //Vérifier le lien d'API, next launch 2022 ?
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
        const hours = Math.floor(diff / (1000 * 60 * 60)) % 24; //Afficher selon le format horaire
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
  