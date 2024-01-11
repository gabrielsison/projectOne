<script setup>
import { ref, onMounted } from 'vue'

import 'leaflet/dist/leaflet.css'
import Leaflet from 'leaflet'
import 'leaflet-rotatedmarker' //here import the plugin

import plane from '../../images/plane.png'

const mapElement = ref(null)
const L = Leaflet
const icon = L.icon({
   iconUrl: plane,
   iconSize: [60, 60], // size of the icon
   iconAnchor: [0, 0], // point of the icon which will correspond to marker's location
})

let direction = 0
let currentCoordinates = { latitude: 51.509865, longitude: -0.118092 } // Initial coordinates for London
let coordinates = []

function generateNextCoordinate(currentCoordinates, speed, direction) {
   const earthRadius = 6371 // Earth radius in kilometers
   const angularDistance = (speed / earthRadius) * (180 / Math.PI) // Convert speed to angular distance
   const newLatitude =
      currentCoordinates.latitude +
      angularDistance * Math.cos((direction * Math.PI) / 180)
   const newLongitude =
      currentCoordinates.longitude +
      angularDistance * Math.sin((direction * Math.PI) / 180)

   return { latitude: newLatitude, longitude: newLongitude }
}

function simulateGPSTracking(fg, map) {
   const speed = 1
   direction = Math.random() * 360

   const nextCoordinates = generateNextCoordinate(
      currentCoordinates,
      speed,
      direction,
   )

   fg.clearLayers()

   currentCoordinates = nextCoordinates

   new L.marker([currentCoordinates.latitude, currentCoordinates.longitude], {
      icon: icon,
      rotationAngle: direction,
   }).addTo(fg)

   map.setView([currentCoordinates.latitude, currentCoordinates.longitude])
}

onMounted(() => {
   var map = L.map(mapElement.value).setView(
      [currentCoordinates.latitude, currentCoordinates.longitude],
      17,
   )

   L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution:
         '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
   }).addTo(map)

   var fg = L.featureGroup().addTo(map)

   const polylinePoints = coordinates

   simulateGPSTracking(fg, map)

   L.polyline(polylinePoints).addTo(map)

   setInterval(() => simulateGPSTracking(fg, map), 10000)
})
</script>

<template>
   <div id="map" ref="mapElement" class="grow min-h-80"></div>
</template>
