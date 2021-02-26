<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Map v-bind:places="places" @select-marker="selectMarker" />
  <div class="bg-gray-100 dark:bg-gray-900 dark:text-white rounded-xl card shadow-lg" v-if="selectedPlace != null"
    :class="{ collapsed: collapsed }">
    <div class="flex-auto p-6 pb-0">
    <div class="flex flex-wrap">
      <h1 class="flex-auto text-xl font-semibold">
      {{ selectedPlace.title }}
      </h1>
      <div class="text-xl font-semibold text-gray-500">
        <a class="chevron md:hidden" href="#" v-on:click="toggleColapse">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" width="30px" height="30px">
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </a>
        <span class="hidden md:inline" v-html="selectedPlace.time" />
      </div>
    </div>
    <div class="pt-0 desc md:hidden">
      <div class="text-xl font-semibold text-gray-500" v-html="selectedPlace.time" />
    </div>
    <div class="pt-6 space-y-4 desc">
    <p class="mb-4">is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.</p>
    </div>
    </div>
    <div class="bg-gray-200 dark:bg-gray-700 rounded-xl rounded-t-none">
      <audio-player :url="`audio/${selectedPlace.audio}`" />
    </div>
  </div>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import { Icon } from 'leaflet';
import AudioPlayer from './components/AudioPlayer.vue';
import { latLng } from "leaflet";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

export default {
  name: 'App',
  components: {
    Map,
    AudioPlayer,
  },
  data() {
    return {
      places: [
        {
          id: 1,
          title: "Flüehlen Talstation",
          loc: latLng(46.893316738836354, 8.627577518254629),
          time: "15:00",
          audio: "walking2.wav"
        },
        {
          id: 2,
          title: "Altdorf Tellstatue",
          loc: latLng(46.881912189415374, 8.644006763776304),
          time: "9:30",
          audio: "walking3.wav"
        }
      ],
      selectedPlace: null,
      collapsed: false,
    }
  },
  methods: {
    selectMarker(p) {
      this.selectedPlace = p;
    },
    toggleColapse() {
      this.collapsed = !this.collapsed;
    }
  }
}
</script>

<style>
html, body  {
  height: 100%;
}



.card {
  text-align: left;
  position: fixed;  
  z-index: 200; 
  background-color: white;
  bottom: 0;
  left: 0;
  width: 100%;
}
@media (min-width: 768px) { 
  .card {
    top: 10px; 
    left: 50px; 
    bottom: auto;
    width: 500px;
  }
}

.card .desc {
  transition: max-height 0.25s ease-in;
  max-height: 100vh;
  overflow: hidden;
}
.card.collapsed .desc {
  max-height: 0px;
}

.card .chevron svg {
  transition: transform 0.35s linear;
}
.card.card.collapsed .chevron svg {
  transform: rotate(180deg);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
