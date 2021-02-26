<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Map v-bind:places="places" @select-marker="selectMarker" />
  <div class="bg-gray-100 dark:bg-gray-900 dark:text-white rounded-xl card shadow-lg" v-if="selectedPlace != null">
    <div class="flex-auto p-6">
    <div class="flex flex-wrap">
      <h1 class="flex-auto text-xl font-semibold">
      {{ selectedPlace.title }}
      </h1>
      <div class="text-xl font-semibold text-gray-500">
        {{ selectedPlace.time }}
      </div>
    </div>
    <div class="pt-6 space-y-4">
    <p>is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.</p>
    </div>
    </div>
    <div class="pb-4 bg-gray-200 dark:bg-gray-700 rounded-xl rounded-t-none">
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
      selectedPlace: null
    }
  },
  methods: {
    selectMarker(p) {
      this.selectedPlace = p;
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
  top: 10px; 
  left: 50px; 
  width: 500px; 
  z-index: 200; 
  background-color: white;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
