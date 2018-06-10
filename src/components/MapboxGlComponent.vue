<template>
  <div class="mymap" ref="map">

  </div>
</template>

<script>
import mapboxgl from 'mapbox-gl';

mapboxgl.accessToken = 'pk.eyJ1IjoidGhpYmF1dGciLCJhIjoiMzg5MzQzOGMwYTJmOGNlODRkYzJjMDc5ZDEwNjNkOTEifQ.YPJ4ALWlOKnLPeaS5Ose7g';

export default {
  data() {
    return {
      map: {},
      centerLngLat: [-96, 37.8],
      usStatesGeojson: {},
    };
  },
  mounted() {
    this.initMap();
    this.addPopup();
    this.addStates();
  },
  methods: {
    initMap() {
      this.map = new mapboxgl.Map({
        container: this.$refs.map,
        style: 'mapbox://styles/mapbox/light-v9',
        center: this.centerLngLat,
        zoom: 3,
      });
    },
    addPopup() {
      const popup = new mapboxgl.Popup()
        .setHTML('<h2>Hello</h2><p>Ca va</p>');
      const marker = new mapboxgl.Marker()
        .setLngLat(this.centerLngLat)
        .setPopup(popup);
      marker.addTo(this.map);
    },
    addStates() {
      this.map.on('load', () => {
        this.map.addLayer({
          id: 'states',
          type: 'fill-extrusion',
          source: {
            type: 'geojson',
            data: './json/us-states.json',
          },
          layout: {},
          paint: {
            'fill-extrusion-color': '#4682b4',
            'fill-extrusion-opacity': 0.7,
            'fill-extrusion-height': ['*', 1000, ['get', 'density']],
          },
        });
      });
    },
  },
};
</script>

<style>
  .mymap {
    height: 600px;
    width: 900px;
    margin: 0 auto;
    text-align: left;
  }
</style>
