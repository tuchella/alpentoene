<template>
  <div id="app" class="font-sans">
    <div class="header bg-white dark:bg-gray-900 shadow flex" style="">
        <div class="">
          <a href="https://www.alpentoene.ch/">
        <img style="height:100%; max-height: 50px;" src="images/atlogo.svg" />
          </a>
        </div>
        <div class="flox-grow flex-1 text-left">
        <span class="text-2xl font-semibold" style="color: #999; display: inline-block; margin-left:0.2em; margin-top: 0.4em;">am dritten tag</span> 
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
    <div class="flex-auto pb-0">
    <div class="flex flex-wrap px-6 pt-4">
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
    <div class="pt-0 desc md:hidden px-6">
      <div class="text-xl font-semibold text-gray-500" v-html="selectedPlace.time" />
    </div>
    <div class="py-4 max-height: 285px; overflow: hidden;">
      <img style="width:100%" :src="selectedPlace.image" />
    </div>
    <div class="px-6 py-4 space-y-4 desc text-justify">
    <p class="mb-4">{{ selectedPlace.description }}</p>
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
          title: "Flüehlen, Ufer",
          loc: latLng(46.901563, 8.621641),
          time: "11:50",
          audio: "walking3.wav",
          description: "Die Wellen schlagen gegen das Ufer. Metall klappert im Wind. Ein Schiff kommt an. Laute Gespräche über die Fahrt und das was danach geschieht.",
          image: "images/fln.jpg"
        },
        {
          id: 2,
          title: "Altdorf, Tellstatue",
          loc: latLng(46.881912189415374, 8.644006763776304),
          time: "17:30",
          audio: "walking2.wav",
          description: "Feierabend. Reges treiben und letzte Besorgungen. Die Terassen sind wieder geöffnet. Überdeckt vom Strassenlärm. Das Plätschern eines Brunnens gerade noch hörbar.",
          image: "images/ts.jpg"
        },
        {
          id: 3,
          title: "Haldi, Wald",
          loc: latLng(46.863253, 8.672428),
          time: "15:20",
          audio: "walking2.wav",
          description: "Die Sonne scheint (nicht hörbar). Vogelgezwitscher und Blätter im Wind. Man hört Kinder in der Ferne spielen, wenn die Kettensäge mal pause macht.",
          image: "images/haldi.jpg"
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
    top: 7rem; 
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
  padding: 0px;
}

.card .chevron svg {
  transition: transform 0.35s linear;
}
.card.card.collapsed .chevron svg {
  transform: rotate(180deg);
}

.card img {
  max-height: 285px;
  transition: max-height 0.1s linear;
}

.card.collapsed img {
  max-height: 0px;
  padding: 0px;
}

.header {
  width:100%; 
  position: fixed; 
  left: 0; 
  top: 0; 
  z-index: 90; 
  height: 6.6rem;
  padding: 0 3rem;
}

.header > div {
  display: flex;
  align-items: center;
} 

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
