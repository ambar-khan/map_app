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
  position: fixed;
  top: 50%;
  left: 50%;
  margin-top: -300px;
  margin-left: -400px;
  width: 800px;
  height: 500px;
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
    var monument = [-118.5649, 34.0459];
    var map = new mapboxgl.Map({
      container: "map", // container id
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: monument, // starting position [lng, lat]
      zoom: 15, // starting zoom
      pitch: 45,
      bearing: -17.6,
      antialias: true,
    });

    // The 'building' layer in the mapbox-streets vector source contains building-height
    // data from OpenStreetMap.
    map.on("load", function () {
      // Insert the layer beneath any symbol layer.
      var layers = map.getStyle().layers;

      var labelLayerId;
      for (var i = 0; i < layers.length; i++) {
        if (layers[i].type === "symbol" && layers[i].layout["text-field"]) {
          labelLayerId = layers[i].id;
          break;
        }
      }

      map.addLayer(
        {
          id: "3d-buildings",
          source: "composite",
          "source-layer": "building",
          filter: ["==", "extrude", "true"],
          type: "fill-extrusion",
          minzoom: 15,
          paint: {
            "fill-extrusion-color": "#aaa",

            // use an 'interpolate' expression to add a smooth transition effect to the
            // buildings as the user zooms in
            "fill-extrusion-height": [
              "interpolate",
              ["linear"],
              ["zoom"],
              15,
              0,
              15.05,
              ["get", "height"],
            ],
            "fill-extrusion-base": [
              "interpolate",
              ["linear"],
              ["zoom"],
              15,
              0,
              15.05,
              ["get", "min_height"],
            ],
            "fill-extrusion-opacity": 0.6,
          },
        },
        labelLayerId
      );
    });

    // create the popup
    var popup = new mapboxgl.Popup({ offset: 25 }).setText(
      "In 1954, oil tycoon J. Paul Getty opened a gallery adjacent to his home in Pacific Palisades.[3][4][5] Quickly running out of room, he built a second museum, the Getty Villa, on the property down the hill from the original gallery.[4][6] The villa design was inspired by the Villa of the Papyri at Herculaneum[."
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
