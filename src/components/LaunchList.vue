<template>
  <div class="launch-list">
    <ul class="timeline">
      <transition-group name="fade" tag="div">
        <li
          v-for="(launch, index) in launches"
          :key="launch.id"
          @click="openLaunchModal(launch)"
          :class="['timeline-item', index % 2 === 0 ? 'left' : 'right']"
        >
          <!-- On regroupe l'image, le titre et la date dans un conteneur vertical -->
          <!-- Pas de patch pour le filtre "Tous les lancements" ?? -->
          <div class="flex flex-col items-center mb-2">
            <img
              v-if="launch.links?.patch?.small"
              :src="launch.links.patch.small"
              alt="Patch de lancement"
              class="w-16 h-16 rounded-full transition transform hover:scale-105 mb-2"
            />
            <h3 class="font-semibold text-white">
              {{ launch.name }}
            </h3>
            <p class="text-sm text-white">
              {{ new Date(launch.date_utc).toLocaleDateString() }}
            </p>
          </div>
        </li>
      </transition-group>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch, onMounted } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'LaunchList',
  props: {
    filter: {
      type: String,
      default: 'all'
    }
  },
  emits: ['launchSelected'],
  setup(props, { emit }) {
    const launches = ref<any[]>([]);

    const fetchLaunches = async () => {
      try {
        const { data } = await axios.get('https://api.spacexdata.com/v5/launches');
        let sortedData = data.sort(
          (a: any, b: any) =>
            new Date(b.date_utc).getTime() - new Date(a.date_utc).getTime()
        );
        if (props.filter === 'success') {
          sortedData = sortedData.filter((launch: any) => launch.success === true);
        } else if (props.filter === 'failure') {
          sortedData = sortedData.filter((launch: any) => launch.success === false);
        }
        launches.value = sortedData.slice(0, 10);
      } catch (error) {
        console.error("Erreur lors de la récupération des lancements :", error);
      }
    };

    watch(() => props.filter, () => {
      fetchLaunches();
    });

    onMounted(() => {
      fetchLaunches();
    });

    const openLaunchModal = (launch: any) => {
      emit('launchSelected', launch);
    };

    return { launches, openLaunchModal };
  }
});
</script>

<style scoped>
.timeline {
  position: relative;
  width: 80%;
  margin: 2rem auto;
  list-style: none;
  padding: 0;
}

.timeline::before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 2px;
  background-color: #ffffff;
}

/* Chaque item occupe 40% et est positionné selon la classe .left ou .right */
.timeline-item {
  position: relative;
  width: 40%;
  padding: 1rem;
  margin-bottom: 2rem;
  cursor: pointer;
}

/* Les items de gauche */
.timeline-item.left {
  left: 10%;
  text-align: right;
}

/* Les items de droite */
.timeline-item.right {
  left: 50%;
  text-align: left;
}

/* Point sur la ligne */
.timeline-item::before {
  content: "";
  position: absolute;
  top: 1.2rem;
  width: 12px;
  height: 12px;
  background-color: #ffffff;
  border-radius: 50%;
}

/* Point à gauche */
.timeline-item.left::before {
  right: -6px;
}

/* Point à droite */
.timeline-item.right::before {
  left: -6px;
}

/* Transition fade */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
