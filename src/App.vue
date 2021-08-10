<template>
<div class="root-tile">
      <div style="position:fixed; top: 10px; right: 0">
        <day-night-toggle v-model="isNight" />
      </div>
      <div class="content-tile">
        <div class="top-content-tile">
          <div class="top-content-col">
            <div class="title-tile">
              <img src="images/atlogo.svg" >  
              <span>am dritten tag</span>
              <span>{{ userAgent }}</span>
            </div>
            <div :class="'location' + (selectedPlace.id == 1 ? ' active' : '')" @click="clickPlace(1)">
              <div class="title" style="background-image: url('images/fln.jpg'); ">
                <span>Flüelen, Ufer</span>
              </div>
              <div class="desc" >
                <span class="desc-text">
                  Die Wellen schlagen gegen das Ufer. Metall klappert im Wind. Ein Schiff kommt an. Laute Gespräche über die Fahrt und das was danach geschieht.
                </span>
                <span class="desc-time">
                  11:00
                </span>
              </div>
            </div>
            <div class="live-date-tile">
              <div style="text-align:right; margin: 1em; color: #ee5b53;">
                LIVE
              </div>
              <div>
                Freitag, 13.August 2021<br>
                23:30 Uhr @ Cinema Leuzinger<br>
                <span style="color:gray">(Eintritt frei)</span>
              </div>
            </div>
          </div>
          <div class="top-content-col">
            <div :class="'location' + (selectedPlace.id == 2 ? ' active' : '')" @click="clickPlace(2)">
              <div class="title" style="background-image: url('images/ts.jpg');">
                <span>Altdorf, Tellstatue</span>
              </div>
              <div class="desc">
                
                <span class="desc-text">
                Feierabend. Reges treiben und letzte Besorgungen. Die Terassen sind wieder geöffnet. Überdeckt vom Strassenlärm. Das Plätschern eines Brunnens gerade noch hörbar.
              </span>
              <span class="desc-time">
                11:00
              </span>
              </div>
            </div>
            
            <div :class="'location' + (selectedPlace.id == 3 ? ' active' : '')" @click="clickPlace(3)">
              <div class="title" style="background-image: url('images/haldi.jpg');">
                <span>Haldi, Wald</span>
              </div>
              <div class="desc">
                
                <span class="desc-text">
                Die Sonne scheint (nicht hörbar). Vogelgezwitscher und Blätter im Wind. Man hört Kinder in der Ferne spielen, wenn die Kettensäge mal pause macht.
              </span>
              <span class="desc-time">
                11:00
              </span> 
              </div>
            </div>
          </div>
        </div>
        <div class="bottom-content-tile">
          <audio-player :url="selectedPlace.audio" />

          
        </div>
        
      </div>
      <div class="map-tile">
        <Map
          v-bind:places="places"
          v-bind:isNight="isNight"
          @select-marker="selectMarker"
        />
      </div>
    </div>
  <!--<div id="app" class="font-sans" style="width: 100vw; height: 100vh; display: flex; flex-direction:column; text-align:justify;">
    <div class="header bg-white dark:bg-gray-900 shadow flex" style="">
      <div class="">
        <a href="https://www.alpentoene.ch/">
          <img style="height: 100%; max-height: 50px" src="images/atlogo.svg" />
        </a>
      </div>
      <div class="flox-grow flex-1 text-left">
        <span
          class="text-2xl font-semibold"
          style="
            color: #999;
            display: inline-block;
            margin-left: 0.2em;
            margin-top: 0.4em;
          "
          >am dritten tag</span
        >
      </div>
      <div class=""></div>
      <div class="text-right">
        <day-night-toggle v-model="isNight" />
      </div>
    </div>
    <div style="background-color:red; flex-grow: 1; display: flex; flex-direction:row; align-items: stretch; height: 100%;">
      <div style="background-color:white; min-width: 200px; display:flex; flex-direction:column; align-items: stretch;  height: 100%" >
        <div style="max-width:500px; font-size: 1.4em">
          <img src="images/fln.jpg" style="width:100%">
          <h3 style="font-size:2.4em">Flülen, Ufer</h3>
          <p>11:30</p>
          <p>
           Die Wellen schlagen gegen das Ufer. Metall klappert im Wind. Ein Schiff kommt an. Laute Gespräche über die Fahrt und das was danach geschieht.
          </p>
        </div>
              
        <div style="overflow:hidden display:flex; flex-grow: 1;">
          <div style="background-image: url(images/haldi.jpg); flex-grow: 1;">
            <h3>Haldi, Bergstation</h3>
          </div>
        </div>
        <div style="overflow:hidden; flex-grow: 1; display:flex">
          <div style="background-image: url(images/ts.jpg); flex-grow: 1;">
            <h3 style="color:white; font-size: 3em;">Haldi, Bergstation</h3>
          </div>
        </div>


      </div>
      <div style="background-color:blue; flex-grow: 1;">
        <Map
          v-bind:places="places"
          v-bind:isNight="isNight"
          @select-marker="selectMarker"
        />
      </div>
    </div>
    <div style="display: none">
      
    </div>
    <div
      class="bg-gray-100 dark:bg-gray-900 dark:text-white rounded-xl card shadow-lg"
      v-if="selectedPlace != null"
      :class="{ collapsed: collapsed }"
    >
      <div class="flex-auto pb-0">
        <div class="flex flex-wrap px-6 pt-4">
          <h1 class="flex-auto text-xl font-semibold">
            {{ selectedPlace.title }}
          </h1>
          <div class="text-xl font-semibold text-gray-500">
            <a class="chevron md:hidden" href="#" v-on:click="toggleColapse">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
                width="30px"
                height="30px"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </a>
            <span class="hidden md:inline" v-html="selectedPlace.time" />
          </div>
        </div>
        <div class="pt-0 desc md:hidden px-6">
          <div
            class="text-xl font-semibold text-gray-500"
            v-html="selectedPlace.time"
          />
        </div>
        <div class="py-4 max-height: 285px; overflow: hidden;">
          <img style="width: 100%" :src="selectedPlace.image" />
        </div>
        <div class="px-6 py-4 space-y-4 desc text-justify">
          <p class="mb-4">{{ selectedPlace.description }}</p>
        </div>
      </div>
      <div class="bg-gray-200 dark:bg-gray-700 rounded-xl rounded-t-none">
        <audio-player :url="selectedPlace.audio" />
      </div>
    </div>
  </div>-->
</template>

<script>
import { Icon } from "leaflet";
import { latLng } from "leaflet";

import AudioPlayer from "./components/AudioPlayer.vue";
import DayNightToggle from "./components/DayNightToggle.vue";
import Map from "./components/Map.vue";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  name: "App",
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
          title: "Flüelen, Ufer",
          loc: latLng(46.901563, 8.621641),
          time: "11:50",
          audio: "Shore.mp3",
          description:
            "Die Wellen schlagen gegen das Ufer. Metall klappert im Wind. Ein Schiff kommt an. Laute Gespräche über die Fahrt und das was danach geschieht.",
          image: "images/fln.jpg",
        },
        {
          id: 2,
          title: "Altdorf, Tellstatue",
          loc: latLng(46.881912189415374, 8.644006763776304),
          time: "17:30",
          audio: "City.mp3",
          description:
            "Feierabend. Reges treiben und letzte Besorgungen. Die Terassen sind wieder geöffnet. Überdeckt vom Strassenlärm. Das Plätschern eines Brunnens gerade noch hörbar.",
          image: "images/ts.jpg",
        },
        {
          id: 3,
          title: "Haldi, Wald",
          loc: latLng(46.863253, 8.672428),
          time: "15:20",
          audio: "Mountains.mp3",
          description:
            "Die Sonne scheint (nicht hörbar). Vogelgezwitscher und Blätter im Wind. Man hört Kinder in der Ferne spielen, wenn die Kettensäge mal pause macht.",
          image: "images/haldi.jpg",
        },
      ],
      selectedPlace: {},
      collapsed: false,
      isNight: false,
    };
  },
  computed: {
    userAgent() {
      return navigator.userAgent;
    }
  },
  methods: {
    selectMarker(p) {
      this.selectedPlace = p;
    },
    clickPlace(id) {
      this.places.forEach(p => {
        if (p.id == id) {
         this.selectMarker(p); 
        }
      })
    },
    toggleColapse() {
      this.collapsed = !this.collapsed;
    },
  },
};
</script>

<style>

* {
  box-sizing: border-box;
}
html {
  background-color: black;
  font-family: "Montserrat", sans-serif;
}
body {
  background-image: url('../public/images/bg_uri2.jpg');
  background-size: auto 100%;
  height: 100vh; 
  width: 100vw; 
  margin: 0; 
  display: flex; 
  align-items: center; 
  justify-content: center;
}
.root-tile div {
  color: black;
  transition: all 0.1s;
}

.dark .root-tile div {
  color: white;
}



.location {
  border: 1px solid white;
  flex-grow: 2;
  flex-shrink: 1;
  filter: grayscale(100%);
  display: flex;
  flex-direction: column;
  max-height: 300px;
}

.location.active {
  filter: grayscale(0%);
  border: 4px solid #ee5b53;
  background-color: #ee5b53;
  color: white;
  flex-shrink: unset;
  max-height: 800px;
}

.location .desc {
  max-height: 0px;
  display: flex;
  overflow: hidden;
}
.location.active .desc {
  max-height: 800px;
}

.location.active .title {
  height: 282px;
}

.location .title {
  max-height: 300px; 
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
  background-size: 120% auto;
}
.location .title span {
  color:white; 
  font-size: 2em;
  text-align: center;
}


.location .desc span {
  color: white;
  margin: 1em;
  font-size: 0.9em;
}

.map-tile {
  background-color: white; 
  border: 1px solid gray; 
  flex-basis:100%; 
  flex-shrink: 0;
  flex-grow: 1;
}

.dark .map-tile {
    background-color: black; 
  border-color: white; 
}

.live-date-tile {
  flex-grow: 1; background-color: white; border: 1px solid gray;
             padding: 1em; display: flex; align-items: center; justify-content: center;
}

.dark .live-date-tile {
  background-color: black;
  border-color: white;
}

.root-tile {
  max-width: 1300px; 
  flex-grow: 1; 
  display: flex; 
  align-items: stretch; 
  flex-wrap: wrap;
}

.content-tile {
  flex-grow: 2; 
  display: flex; 
  flex-direction: column; 
  overflow: hidden;
}
.top-content-tile {
  flex-grow: 1; display: flex; flex-direction: row; 
}
.top-content-col {
  width: 50%; display: flex; flex-direction: column; align-items: stretch; justify-content: space-between;
}

.bottom-content-tile {
  height: 136px; 
  background-color: white; 
  border: 1px solid gray; 
  flex-shrink: 0;
}

.dark .bottom-content-tile {
  background-color: black;
  border-color: white;
}

.title-tile {
  background-color: white; 
  border: 1px solid gray; 
  max-height: 150px;
              padding: 1em; display: flex; align-items: center; justify-content: space-evenly;
}

.dark .title-tile {
  background-color: black; 
  border-color: white;
}

.title-tile img {
  max-width: 250px; width: 70%;
}

@media only screen and (min-width: 600px)  {
  .map-tile {
    flex-basis:33%; 
    flex-grow: 0;
  } 
  .root-tile {
    flex-wrap: nowrap;
    min-height: 730px; 
  }
}
</style>
