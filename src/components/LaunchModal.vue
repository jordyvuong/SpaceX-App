<template>
  <div 
  v-if="launch" 
  class="fixed inset-0 flex justify-center items-center z-50"
  @click.self="closeModal"
  >
    <div class="absolute inset-0 bg-opacity-20 pointer-events-none"></div>
    <div class="relative bg-white bg-opacity-90 p-6 rounded-lg w-11/12 max-w-2xl">
      <button @click="closeModal" class="absolute top-2 right-2 text-gray-600 hover:text-gray-800">X</button>
      <h2 class="text-2xl font-bold mb-2">{{ launch.name }}</h2>
      <p>Date : {{ formattedDate }}</p>
      <p class="my-4">{{ launch.details || 'Aucune description disponible.' }}</p>
      <img
        v-if="launch.links && launch.links.patch && launch.links.patch.small"
        :src="launch.links.patch.small"
        alt="Illustration mission"
        class="my-4 mx-auto"
      />
      <a
        v-if="launch.links && launch.links.article"
        :href="launch.links.article"
        target="_blank"
        class="text-blue-500 underline"
      >
        Lire l'article de présentation
      </a>
      <div class="mt-4">
        <label class="flex items-center">
          <input type="checkbox" v-model="showVideo" class="mr-2" />
          Afficher la vidéo YouTube
        </label>
      </div>
      <div v-if="showVideo" class="mt-4">
        <iframe
          v-if="youtubeUrl"
          width="560"
          height="315"
          :src="youtubeUrl"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
        <p v-else>Aucune vidéo disponible.</p>
      </div>
      <div class="mt-4">
        <p><strong>Lieu de lancement :</strong> {{ launchpadName }}</p>
        <p><strong>Payloads :</strong> {{ payloadsNames }}</p>
        <p><strong>Clients :</strong> {{ clientsNames }}</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'LaunchModal',
  props: {
    launch: {
      type: Object,
      default: null
    }
  },
  emits: ['close'],
  setup(props, { emit }) {
    const showVideo = ref(false);
    const launchpads = ref<{ [key: string]: string }>({});

    const fetchLaunchpads = async () => {
      try {
        const response = await axios.get('https://api.spacexdata.com/v4/launchpads');
        const launchpadData = response.data;
        launchpadData.forEach((launchpad: any) => {
          launchpads.value[launchpad.id] = launchpad.name;
        });
      } catch (error) {
        console.error('Erreur lors de la récupération des launchpads:', error);
      }
    };

    onMounted(() => {
      fetchLaunchpads();
    });

    const youtubeUrl = computed(() => {
      return props.launch && props.launch.links.youtube_id
        ? `https://www.youtube.com/embed/${props.launch.links.youtube_id}`
        : '';
    });

    const formattedDate = computed(() => {
      if (!props.launch) return '';
      const date = new Date(props.launch.date_utc);
      return date.toLocaleDateString();
    });

    const payloadsNames = computed(() => {
      if (!props.launch || !props.launch.payloads || props.launch.payloads.length === 0) {
        return 'Aucun payload disponible';
      }
      return props.launch.payloads.map((p: any) => p.name).join(', ');
    });

    const clientsNames = computed(() => {
      if (!props.launch || !props.launch.payloads || props.launch.payloads.length === 0) {
        return 'Aucun client disponible';
      }
      const customers = props.launch.payloads.flatMap((p: any) => p.customers || []);
      return customers.length > 0 ? Array.from(new Set(customers)).join(', ') : 'Aucun client disponible';
    });

    const launchpadName = computed(() => {
      return launchpads.value[props.launch.launchpad] || props.launch.launchpad;
    });

    const closeModal = () => {
      emit('close');
    };

    return { showVideo, youtubeUrl, formattedDate, payloadsNames, clientsNames, launchpadName, closeModal };
  }
});
</script>

<style scoped>
</style>
