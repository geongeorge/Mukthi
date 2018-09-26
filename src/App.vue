<template>
  <div id="app">
    <mapbox
 access-token="pk.eyJ1IjoiZ2Vvbmdlb3JnZWsiLCJhIjoiY2prZm5yaWZ5MDluODNrczdrMHRhZHB5eCJ9.xdRyfEKi2zGVLEUHIiVgFA"
:map-options="{
  style: 'mapbox://styles/mapbox/light-v9',
  center: [-96, 37.8],
  zoom: 3
}"
:geolocate-control="{
  show: true,
  position: 'top-left'
}"
:scale-control="{
  show: true,
  position: 'top-left'
}"
:fullscreen-control="{
  show: true,
  position: 'top-left'
}">
</mapbox>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Mapbox from 'mapbox-gl-vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
    Mapbox
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}
#map {
  width: 100%;
  height: 500px;
}
</style>
