<template>
  <div class="home">
    <div id="map"></div>  
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
}

#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 500px;
  height: 300px;
}
</style>

<script>
export default {
  name: "Home",
  components: {},
  mounted: function () {
    // console.log(process.env.VUE_APP_MY_API_KEY);
    mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
    var map = new mapboxgl.Map({
      container: "map", // container id
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [-74.5, 40], // starting position [lng, lat]
      zoom: 10, // starting zoom
    });
  },
  methods: {},
};
</script>
