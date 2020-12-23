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
/* global mapboxgl */
export default {
  name: "Home",
  components: {},
  mounted: function () {
    // console.log(process.env.VUE_APP_MY_API_KEY);
    mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
    var monument = [-77.0353, 38.8895];
    var map = new mapboxgl.Map({
      container: "map", // container id
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: monument, // starting position [lng, lat]
      zoom: 15, // starting zoom
    });
    // create the popup
    var popup = new mapboxgl.Popup({ offset: 25 }).setText(
      "Construction on the Washington Monument began in 1848."
    );

    // create DOM element for the marker
    var el = document.createElement("div");
    el.id = "marker";

    // create the marker
    new mapboxgl.Marker(el)
      .setLngLat(monument)
      .setPopup(popup) // sets a popup on this marker
      .addTo(map);
  },
  methods: {},
};
</script>
