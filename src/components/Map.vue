
<template>
  <div class="map">
    <day-night-toggle v-model="isNight" />
    <l-map
      ref="map"
      :zoom="zoom"
      :center="center"
      :maxZoom="20"
      style="height: 100vh; width: 100%; position: fixed; inset: 0px; "
    >
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-marker :lat-lng="place.loc" v-for="place in places" :key="place.id" @click="clicked(place)" />
    </l-map>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import * as L from "leaflet";
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import "proj4leaflet";
import "leaflet.tilelayer.colorfilter";
import DayNightToggle from './DayNightToggle.vue';
//import proj4 from "proj4";

/*
const swissCrs = new L.Proj.CRS(
  'EPSG:2056',
  '+proj=somerc +lat_0=46.95240555555556 +lon_0=7.439583333333333 +k_0=1 +x_0=2600000 +y_0=1200000 +ellps=bessel +towgs84=674.374,15.056,405.346,0,0,0,0 +units=m +no_defs',
//  'EPSG:21781',
//  '+proj=somerc +lat_0=46.95240555555556 +lon_0=7.439583333333333 +k_0=1 +x_0=600000 +y_0=200000 +ellps=bessel +towgs84=674.4,15.1,405.3,0,0,0,0 +units=m +no_defs',
  {
    resolutions: [
///      4000, 3750, 3500, 3250, 3000, 2750, 2500, 2250, 2000, 1750, 1500, 1250, 1000, 750, 650, 500, 250, 100, 50, 20, 10, 5, 2.5, 2, 1.5, 1, 0.5
         4000, 3750, 3500, 3250, 3000, 2750, 2500, 2250, 2000, 1750, 1500, 1250, 1000, 750, 650, 500, 250, 100, 50, 20, 10, 5, 2.5, 2,  1.5, 1, 0.5, 0.25, 0.1
    ],
    origin: [2420000, 1350000]
  })
  */

const NIGHT_FILTER = ["hue:180deg", "invert:100%", "grayscale:50%"];
const DAY_FILTER = ["hue:0deg", "invert:0%", "grayscale:0%"];
const filterLayer = L.tileLayer.colorFilter(
        "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
        {
          attribution:
            '<a href="https://wikimediafoundation.org/wiki/Maps_Terms_of_Use">Wikimedia</a>',
          filter: DAY_FILTER,
        }
      );

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    DayNightToggle,
  },
  props: [ "places" ],
  data() {
    return {
      zoom: 14,
      center: latLng(46.881912189415374, 8.644006763776304),
      //      crs: swissCrs,
      //      url: ' https://wmts.geo.admin.ch/1.0.0/ch.swisstopo.pixelkarte-farbe/default/current/21781/{z}/{x}/{y}.jpeg',
      //      url: ' https://wmts.geo.admin.ch/1.0.0/ch.swisstopo.pixelkarte-farbe/default/current/2056/{z}/{x}/{y}.jpeg',
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="https://www.geo.admin.ch/">geo.admin.ch</a>',
      isNight: false
    };
  },
  watch: {
    isNight: function() {
      this.toggleFilter();
    }
  },
  methods: {
    toggleFilter() {
      if (this.isNight) {
        filterLayer.updateFilter(NIGHT_FILTER);
      } else {
        filterLayer.updateFilter(DAY_FILTER);
      }
    },
    clicked(place) {
      this.$emit('select-marker', place)
    } 
  },
  mounted() {
    this.$nextTick(() => {
      filterLayer.addTo(this.$refs.map.mapObject);
    });
  },
};
</script>
