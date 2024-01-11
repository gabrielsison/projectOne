<script setup>
import 'leaflet/dist/leaflet.css'
import Leaflet from 'leaflet'
import { ref, onMounted } from 'vue'

const mapElement = ref(null)
const L = Leaflet

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
   const speed = 0.05 // Adjust speed as needed
   const direction = Math.random() * 360 // Random direction in degrees
   // Function to be called every minute
   const nextCoordinates = generateNextCoordinate(
      currentCoordinates,
      speed,
      direction,
   )
   console.log(
      `GPS Coordinates: Latitude ${nextCoordinates.latitude.toFixed(
         6,
      )}, Longitude ${nextCoordinates.longitude.toFixed(6)}`,
   )

   fg.clearLayers()
   // Update current coordinates for the next iteration
   currentCoordinates = nextCoordinates

   new L.marker([
      currentCoordinates.latitude,
      currentCoordinates.longitude,
   ]).addTo(fg)

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

   setInterval(() => simulateGPSTracking(fg, map), 30000)
})
</script>

<template>
   <div id="map" ref="mapElement" class="grow min-h-80"></div>
</template>
