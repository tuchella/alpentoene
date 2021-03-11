<template>
  <div id="app" class="font-sans">
    <div class="header bg-white dark:bg-gray-900 shadow flex p-2" style="width:100%; position: fixed; left: 0; top: 0; z-index: 90;">
        <div class="">
        <h1 class="text-3xl font-semibold" style="color: #ee5b53; display: inline-block; "><span class="font-ma">a</span>lpentöne</h1>
        <h2 class="text-2xl font-semibold" style="color: #999; display: inline-block; margin-left:0.2em;">am dritten tag</h2>
        </div>
        <div class="flox-grow flex-1">
        </div>
        <div class="">
        </div>
        <div class="text-right">
            <day-night-toggle v-model="isNight" />
        </div>
    </div>
    <Map v-bind:places="places" v-bind:isNight="isNight" @select-marker="selectMarker" />
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
    <p class="mb-4">Hier steht irgendwann mal eine Beschreibung zu dem Ort. Es war schön hier. Vielleicht auch was hier so statt findet. Jeglich Informationen, die man sich nur wünschen kann.</p>
    <p class="mb-4">Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.</p>
    </div>
    </div>
    <div class="bg-gray-200 dark:bg-gray-700 rounded-xl rounded-t-none">
      <audio-player :url="selectedPlace.audio" />
    </div>
  </div>
  </div>
</template>

<script>
import { Icon } from 'leaflet';
import { latLng } from "leaflet";

import AudioPlayer from './components/AudioPlayer.vue';
import DayNightToggle from './components/DayNightToggle.vue';
import Map from './components/Map.vue'

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
    DayNightToggle,
  },
  data() {
    return {
      places: [
        {
          id: 1,
          title: "Flüehlen Talstation",
          loc: latLng(46.893316738836354, 8.627577518254629),
          time: "15:00",
          audio: "walking3.wav"
        },
        {
          id: 2,
          title: "Altdorf Tellstatue",
          loc: latLng(46.881912189415374, 8.644006763776304),
          time: "9:30",
          audio: "walking2.wav"
        }
      ],
      selectedPlace: null,
      collapsed: false,
      isNight: false,
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
  font-family: 'Montserrat', sans-serif;
}

.font-ma {
    font-family: 'Montserrat Alternates', sans-serif;
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
    top: 70px; 
    left: 60px; 
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
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
