<template>
  <l-map style="height: 300px" :zoom="zoom" :center="coords">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    {{coords}}
    <l-marker :lat-lng="coords"></l-marker>
  </l-map>
</template>

<script>

import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import 'leaflet/dist/leaflet.css';
 import L from 'leaflet';

delete L.Icon.Default.prototype._getIconUrl;


L.Icon.Default.mergeOptions({
   iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
   iconUrl: require('leaflet/dist/images/marker-icon.png'),
   shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

export default {
  name: "Map",
  props: ["coords"]
  ,
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 15,
            center: [47.313220, -1.319482],

    };
  },
};
</script>
