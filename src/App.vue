<template>
  <div id="app">
    <img src="./assets/web_logo.png" id="logo">
    <mapbox
:access-token="mapData.token"
:map-options="mapData.options"
:geolocate-control="mapData.geocontrol"
@map-init="mapLoaded">
</mapbox>
<button @click="axiostest">Twest</button>
{{info}}
<div class="columns is-mobile is-gapless yo-bottom">
  <div class="column" v-for="(btn,i) in btnTypes" :key="i">
    <b-button :type="btn"  @wasclicked="wasclicked"></b-button>
  </div>
</div>
  
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Mapbox from 'mapbox-gl-vue';
import BButton from './components/BButton.vue'
import axios from "axios"

export default {
  name: 'app',
  components: {
    HelloWorld,
    Mapbox,
    BButton
  },
  data() {
    return {
      info: "empty",
      mymap: "",
      mapData: {
        token : "pk.eyJ1IjoiZ2Vvbmdlb3JnZWsiLCJhIjoiY2prZm5yaWZ5MDluODNrczdrMHRhZHB5eCJ9.xdRyfEKi2zGVLEUHIiVgFA",
        options: {
                    style: 'mapbox://styles/mapbox/dark-v9',
                    center: [76.9629, 10.7537],
                    zoom: 7
                  },
        geocontrol: {
                      show: false,
                      position: 'top-left'
                    },
                    
        
      },
      btnTypes: [
          "camp",
          "collection",
          "help",
          "road"
        ],
    }
  },
  methods : {
    mapLoaded(map) {
     this.mymap = map
//       map.addLayer({
//         'id': 'points',
//         'type': 'symbol',
//         'source': {
//           'type': 'geojson',
//           'data': {
//   "type": "FeatureCollection",
//   "features": [
//     {
//       "type": "Feature",
//       "properties": {
//         "marker-color": "#ef0e24",
//         "marker-size": "medium",
//         "marker-symbol": ""
//       },
//       "geometry": {
//         "type": "Point",
//         "coordinates": [
//           76.52664184570312,
//           9.596264590393757
//         ]
//       }
//     }
//   ]
// }
//         }
//       });
    },
    // mapLoaded(map) {
    //   console.log(map)
    //   this.mymap = map;
    // },
    axiostest() {
      axios
      .get('http://www.mocky.io/v2/5babb40731000069006544d2')
      .then(response => {
        console.log(response.data)
        this.mymap.addSource("earthquakes", {
        "type": "geojson",
        "data": {
                "type": "FeatureCollection",
                "features": [
                  {
                    "type": "Feature",
                    "properties": {
                      "marker-color": "#ef0e24",
                      "marker-size": "medium",
                      "marker-symbol": ""
                    },
                    "geometry": {
                      "type": "Point",
                      "coordinates": [
                        76.52664184570312,
                        9.596264590393757
                      ]
                    }
                  }
                ]
              }
      })
      this.mymap.addLayer({
        "id": "earthquakes-heat",
        "type": "heatmap",
        "source": "earthquakes",
        "maxzoom": 9,
        "paint": {
            // Increase the heatmap weight based on frequency and property magnitude
            "heatmap-weight": [
                "interpolate",
                ["linear"],
                ["get", "mag"],
                0, 0,
                6, 1
            ],
            // Increase the heatmap color weight weight by zoom level
            // heatmap-intensity is a multiplier on top of heatmap-weight
            "heatmap-intensity": [
                "interpolate",
                ["linear"],
                ["zoom"],
                0, 1,
                12, 6
            ],
            // Color ramp for heatmap.  Domain is 0 (low) to 1 (high).
            // Begin color ramp at 0-stop with a 0-transparancy color
            // to create a blur-like effect.
            "heatmap-color": [
                "interpolate",
                ["linear"],
                ["heatmap-density"],
                0, "rgba(33,102,172,0)",
                0.2, "rgb(103,169,207)",
                0.4, "rgb(209,229,240)",
                0.6, "rgb(253,219,199)",
                0.8, "rgb(239,138,98)",
                1, "rgb(178,24,43)"
            ],
            // Adjust the heatmap radius by zoom level
            "heatmap-radius": [
                "interpolate",
                ["linear"],
                ["zoom"],
                0, 2,
                15, 30
            ],
            // Transition from heatmap to circle layer by zoom level
            "heatmap-opacity": [
                "interpolate",
                ["linear"],
                ["zoom"],
                7, 1,
                9, 0
            ],
        }
    }, 'waterway-label');

    this.mymap.addLayer({
        "id": "earthquakes-point",
        "type": "circle",
        "source": "earthquakes",
        "minzoom": 7,
        "paint": {
            // Size circle radius by earthquake magnitude and zoom level
            "circle-radius": [
                "interpolate",
                ["linear"],
                ["zoom"],
                7, [
                    "interpolate",
                    ["linear"],
                    ["get", "mag"],
                    1, 1,
                    6, 4
                ],
                16, [
                    "interpolate",
                    ["linear"],
                    ["get", "mag"],
                    1, 5,
                    6, 50
                ]
            ],
            // Color circle by earthquake magnitude
            "circle-color": [
                "interpolate",
                ["linear"],
                ["get", "mag"],
                1, "rgba(33,102,172,0)",
                2, "rgb(103,169,207)",
                3, "rgb(209,229,240)",
                4, "rgb(253,219,199)",
                5, "rgb(239,138,98)",
                6, "rgb(178,24,43)"
            ],
            "circle-stroke-color": "white",
            "circle-stroke-width": 1,
            // Transition from heatmap to circle layer by zoom level
            "circle-opacity": [
                "interpolate",
                ["linear"],
                ["zoom"],
                7, 0,
                8, 1
            ]
        }
    }, 'waterway-label');
        
         
      })
    },
    wasclicked(event) {
      console.log(event)
      this.axiostest()
    }
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
  height: 100%;
}
.yo-bottom {
  position: fixed;
  width: 100%;
  left: 0;
  bottom:0;
}
.yo-bottom .column{
  position:relative;
    bottom: 0;
}
.yo-bottom .column div {
  position:absolute;
    bottom: 0;
    width: 100%;
}
#logo {
  position: fixed;
  top: 20px;
  left: 20px;
  width:180px;
  z-index: 2;

}
</style>
