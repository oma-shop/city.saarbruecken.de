<template>
 <div id="map-wrap">
        <client-only>
          <l-map
            class="map"
            :zoom="zoom"
            :center="center"
            @update:zoom="zoomUpdated"
            @update:center="centerUpdated"
            @update:bounds="boundsUpdated"
          >
            <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
            <l-marker :icon="icon" :lat-lng="center">
              <l-popup>
              
              </l-popup>
            </l-marker>

          </l-map>
        </client-only>
      </div> 
</template>

<script>
export default {
  components: {},
  data() {
    return {
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 13,
      center: [process.env.cityLatitude, process.env.cityLongitude],
      bounds: null,
      markers: []
    };
  },
  methods: {
    zoomUpdated(zoom) {
      this.zoom = zoom;
    },
    centerUpdated(center) {
      this.center = center;
      this.$store.commit('location/setLat', center.lat)
      this.$store.commit('location/setLng', center.lng)
      console.log('this.$store.location.lat', this.$store.state.location.lat)
      console.log('this.$store.location.lat', center)
      
    },
    boundsUpdated(bounds) {
      this.bounds = bounds;
    }
  },
  computed: {
    icon() {
        
    if (process.browser) {
      require('vue2-leaflet');
      console.log("th", this)
      console.log("th", L)
      return L.icon({
        iconUrl: require('@/assets/target-marker.svg'),
        iconSize: [40, 40],
        iconAnchor: [20, 20]
      })
        }
    }
  }
};
</script>

<style>
#map-wrap {
  height: 50vh;
  margin-top: 15px;
}
.map {
  height: 100%;
  width: 100%;
}
.spots {
  padding-top: 80px;
  padding-bottom: 20px;
  float: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: center;
  width: 80%;
  display: block !important;
  margin-right: auto !important;
  margin-left: auto !important;
  float: none !important; /* Added */
}
</style>