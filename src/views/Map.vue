<template>
  <div id="app">
    <div style="height: 500px; width: 100%">
      <l-map
        v-if="showMap"
        :zoom="zoom"
        :center="center"
        :options="mapOptions"
        style="height: 80%"
        @update:center="centerUpdate"
        @update:zoom="zoomUpdate"
      >
        <l-tile-layer :url="url" :attribution="attribution" />
        <l-marker v-for="record in infos.records" :lat-lng="record.fields.stop_coordinates" >
          <l-popup>
            <div @click="innerClick">
              {{record.fields.stop_name}}
            </div>
          </l-popup>
        </l-marker>
      </l-map>
    </div>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from "vue2-leaflet";
const apiURL_1 = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_tan-arrets&q=&rows=20'
const apiURL_2 = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_tan-arrets&q=&rows=20&refine.stop_id='

export default {
  name: "App",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LTooltip,
  },
  data() {
    return {
      zoom: 13,
      center: latLng(47.21796816434429, -1.5529648679904837),
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      currentZoom: 11.5,
      currentCenter: latLng(47.21796816434429, -1.5529648679904837),
      mapOptions: {
        zoomSnap: 0.5,
      },
      showMap: true,
      infos: [],
      arrets: [],
    };
  },
  created: function () {
    this.arrets = JSON.parse(localStorage.getItem('listes_arrets'));
    //console.log(this.arrets);
    this.fetchData();
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    innerClick() {
      alert("Click!");
      localStorage.setItem('test', 'test de valeur')
    },
    fetchData: async function () {
        try {

            const response = await this.axios.get(apiURL_1);
            this.infos = response.data;

            //console.log(this.arrets);
            //this.arrets.forEach(async (a) => {
            //    const response = await this.axios.get(apiURL_2 + a);
            //    this.infos = response.data;
            //    console.log(this.infos);
            //})
            //console.log(this.infos)
          } catch (error) {
            console.log(error);
        }
    }
  },
};
</script>
