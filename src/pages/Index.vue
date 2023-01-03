<template>
  <q-page padding class="flex">
    <q-header class="elevated  z-top height-hint='64'">
      <q-toolbar class="bg-primary glossy text-white ">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          
        />
        <!-- @click="drawerLeft = !drawerLeft" -->

        <q-toolbar-title>
          Catchment Area Results
        </q-toolbar-title>

        <!-- <div>Quasar v{{ $q.version }}</div> -->
        <div>2022</div>

      </q-toolbar>

      <q-toolbar class="bg-white text-black">
        <q-toolbar-title>
        </q-toolbar-title>

        <!-- <div>Quasar v{{ $q.version }}</div> -->
        <div></div>
      </q-toolbar>
    </q-header>

    <div class="row full-width">
      <div class="q-pa-md col-3"></div>
      <div class="q-pa-md col-9">
        <q-tabs>
        <!-- <q-tabs v-model="tabSelected" shrink @input="tabChanged()"> -->
          <q-tab name="page-Map" label="Map" />
          <q-tab name="page-Data" label="Data" />
          <q-tab name="page-Spreadsheet" label="Spreadsheet" />
          <q-tab name="page-Chart" label="Charts" />
        </q-tabs>

        <div class="q-pa-md" id= "page-Map">
          <div class="text-h4 q-mb-md "   data-mode="" style="display:block;">
            <l-map style="height: 1000px" :zoom="zoom" :center="center" :defaultIcon="defaultIcon">
              <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
              <l-geo-json :geojson="geojson"></l-geo-json>
              <l-marker v-for="marker in markers" 
                :key="marker.id" 
                :visible="marker.visible" 
                :draggable="marker.draggable" 
                :lat-lng="marker.position"  
                :icon = "defaultIcon"></l-marker>
            </l-map>
          </div>
        </div>

        <div class="q-pa-md" id = "page-Data"  >
          <!-- <data_grid></data_grid> -->
        </div>

        <div class="q-pa-md" id = "page-Spreadsheet" >
          <!-- <spreadsheet></spreadsheet> -->
        </div>
          <div class="q-pa-md" id= "page-Chart" >
          <!-- <syn_chart :syncdata="syncData"> ></syn_chart> -->
        </div>
      </div>
    </div>


    <!-- <q-card style="flex:1">
      <l-map :zoom="zoom" :maxZoom="maxZoom" :minZoom="minZoom" :center="center">
        <l-control-layers position="topright"  ></l-control-layers>
        <q-btn-dropdown
          icon="map"
          style="top:0; right: 0; position: absolute; margin:10px 10px 0 0; background-color:white; z-index:800;"
        >
          <q-list>
            <q-item
              :key="index"
              v-for="(layer, index) in layers"
              @click="currentLayer=index"
              clickable
              v-close-popup
            >
              <q-item-section>
                <q-item-label>{{layer.title}}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-btn-dropdown>
        <l-tile-layer
          :url="layers[currentLayer].url"
          :attribution="layers[currentLayer].attribution"
        ></l-tile-layer>
      </l-map>
    </q-card> -->

  </q-page>
</template>

<style>
</style>

<script>
import { LMap, LTileLayer, LMarker, LGeoJson } from 'vue2-leaflet'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
import geoSC from "../assets/geoSouthCarolinaCounties.json";

export default {
  name: 'PageIndex',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LGeoJson
    /* LControlLayers */
  },
  data() {
    return {
      zoom: 8,
      maxZoom: 16,
      minZoom: 3,
      center: L.latLng(33.385586, -80.874394),
      currentLayer: 0,
      layers: [
        {
          title: 'drawing',
          url:
            'https://stamen-tiles-{s}.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.jpg',
          attribution:
            'Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        },
        {
          title: 'wikimedia',
          url: 'https://maps.wikimedia.org/osm-intl/{z}/{x}/{y}{r}.png',
          attribution:
            '<a href="https://wikimediafoundation.org/wiki/Maps_Terms_of_Use">Wikimedia</a>'
        },
        {
          title: 'natgeo',
          url:
            'https://server.arcgisonline.com/ArcGIS/rest/services/NatGeo_World_Map/MapServer/tile/{z}/{y}/{x}',
          attribution:
            'Tiles &copy; Esri &mdash; National Geographic, Esri, DeLorme, NAVTEQ, UNEP-WCMC, USGS, NASA, ESA, METI, NRCAN, GEBCO, NOAA, iPC'
        },
        {
          title: 'satellite',
          url:
            'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
          attribution:
            'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
        }
      ],
      markers: [
                  { id: "m1", position : {lat:34.586956, lng:-82.238054}, tooltip: "tooltip for marker3", draggable: true, visible: true, icon: this.defaultIcon },
                  { id: "m2", position : {lat:34.409676, lng:-82.246337}, tooltip: "tooltip for marker4", draggable: true, visible: true, icon: this.defaultIcon }
              ],              
      defaultIcon: L.icon({
        iconUrl: 'http://leafletjs.com/examples/custom-icons/leaf-green.png',
        shadowUrl: 'http://leafletjs.com/examples/custom-icons/leaf-shadow.png',
        iconSize:     [38, 95],
        shadowSize:   [50, 64],
        iconAnchor:   [22, 94],
        shadowAnchor: [4, 62],
        popupAnchor:  [-3, -76]
      }),
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution: '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      geojson: null
    }
  },

  async created () {
    //   const response = await fetch('https://rawgit.com/gregoiredavid/france-geojson/master/regions/pays-de-la-loire/communes-pays-de-la-loire.geojson');
    this.geojson = geoSC //await response.json();
  },
  mounted() {
    
  }
}
</script>
