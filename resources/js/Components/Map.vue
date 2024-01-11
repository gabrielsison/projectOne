<script setup>
import { ref, onMounted } from 'vue'

import 'leaflet/dist/leaflet.css'
import Leaflet from 'leaflet'
import 'leaflet-rotatedmarker' //here import the plugin

import plane from '../../images/plane.png'

const mapElement = ref(null)
const L = Leaflet
const icon = L.icon({
   iconUrl:
      'data:image/svg+xml;base64,PHN2ZyBpZD0icGxhbmVfMjEiIGhlaWdodD0iNjQwIiBzdHlsZT0idHJhbnNmb3JtOnJvdGF0ZSgxMTZkZWcpIiBwcmVzZXJ2ZUFzcGVjdFJhdGlvPSJ4TWlkWU1pZCBtZWV0IiB2aWV3Qm94PSIwIDAgNjQwIDY0MCIgd2lkdGg9IjY0MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+PGRlZnM+PHBhdGggaWQ9ImEiIGQ9Im0zMjEuNjYgNjA5LjY5YzU3LjI2IDE0LjE4IDg5LjA3IDIyLjA1IDk1LjQ0IDIzLjYzIDUuNzIgMS41NSAxMS4wMy0yLjA3IDExLjg4LTYuNTQuNy0xLjI1LjctMS4yNS4xNC0zLjIxLS4wOC0yLjUxLS43MS0yMi42MS0uNzktMjUuMTMuMTUtMy4yMS0xLjY2LTUuODYtNC4xNy03LjI2LTcuMzYtNS4xLTY2LjI1LTQ1Ljg2LTczLjYxLTUwLjk2LTIuNTEtMS40LTMuNjItNS4zMS0zLjQ3LTguNTIuNzctMTguNSA2Ljk3LTE2Ni40NSA3Ljc1LTE4NC45NS0uNDEtNS4xNyA1LjYtMTAuMDQgMTAuMDctOS4xOSAxLjI1LjcgMy4yMS4xNCAzLjIxLjE0IDI1LjM1IDExLjE5IDIyOC4yMSAxMDAuNyAyNTMuNTcgMTExLjg5IDUuMDIgMi44IDExLjU4LS4xMiAxMy4xMy01Ljg0LjctMS4yNiAxLjQtMi41MS44NS00LjQ3LS4xMi00LjUxLTEuMDYtNDAuNTgtMS4xNy00NS4wOS4xNC0zLjIxLTEuNjctNS44Ny00LjE4LTcuMjctMjYuNTctMTkuOTMtMjM5LjEyLTE3OS4zNi0yNjUuNjgtMTk5LjI5LTIuNTEtMS40LTQuMzItNC4wNi00LjE4LTcuMjctLjQ3LTQ2LjM0LTMuMjctMTEyLjEtNC42NS0xMjQuMzktMS4wMi00OC4zLTcwLjMxLTk1LjE5LTc5LjE3IDE1LjA3LS41OSAxMi44NC45NiA3Ny45IDMuMjcgMTEyLjEuNTUgMS45Ni0uODUgNC40Ny0zLjUgNi4yOC0yNi41NCAyMC40MS0yMzguODEgMTgzLjcyLTI2NS4zNSAyMDQuMTMtMS40IDIuNTEtMi44IDUuMDItMi45NCA4LjIzLS4wMSA0LjQ0LS4wOCAzOS45Ni0uMDkgNDQuNC0xLjU0IDUuNzIgMi43OCA5Ljc3IDkuMiAxMC4wNiAxLjI1LjcgMy4yMS4xNSAzLjkxLTEuMSAyNS40My0xMS42NSAyMjguODYtMTA0Ljg1IDI1NC4yOC0xMTYuNSA0LjYyLTIuMzYgMTAuMzQtLjgyIDEyIDUuMDUuNTUgMS45Ni41NSAxLjk2IDEuMTEgMy45MS43IDE4LjUgNi4zNCAxNjYuNDggNy4wNCAxODQuOTgtLjE0IDMuMjEtMS41NCA1LjcyLTQuOSA4Ljc4LTcuMzggNS4yNy02Ni40MyA0Ny4zNy03My44MSA1Mi42My0yLjY1IDEuODEtNC4wNSA0LjMyLTQuMiA3LjUzLS4wNSAyLjQ0LS40MiAyMS45OC0uNDYgMjQuNDMuNCA1LjE2IDQuMDIgMTAuNDcgOS4xOSAxMC4wNyAxLjI1LjcgMS45NS0uNTYgMy4yMS4xNCA2LjQ3LTEuNzYgMzguODMtMTAuNTkgOTcuMDctMjYuNDd6Ii8+PC9kZWZzPjx1c2UgZmlsbD0iIzMzNWVlYSIgeGxpbms6aHJlZj0iI2EiIHN0cm9rZT0iI2ZmZmZmZiIgc3Ryb2tlLXdpZHRoPSIyJSIgLz48dXNlIGZpbGw9Im5vbmUiIHhsaW5rOmhyZWY9IiNhIi8+PC9zdmc+',
   iconSize: [35, 35], // size of the icon
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

   let _marker = new L.marker(
      [currentCoordinates.latitude, currentCoordinates.longitude],
      {
         icon: icon,
         rotationAngle: direction,
      },
   ).addTo(fg)

   map.flyTo(_marker.getLatLng(), 12)
}

onMounted(() => {
   var map = L.map(mapElement.value, { worldCopyJump: false }).setView(
      [currentCoordinates.latitude, currentCoordinates.longitude],
      13,
   )

   L.tileLayer(
      'http://services.arcgisonline.com/arcgis/rest/services/Ocean/World_Ocean_Base/MapServer/tile/{z}/{y}/{x}',
      {
         maxZoom: 13,
         attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
      },
   ).addTo(map)

   var fg = L.featureGroup().addTo(map)

   const polylinePoints = coordinates

   simulateGPSTracking(fg, map)

   L.polyline(polylinePoints).addTo(map)

   setInterval(() => simulateGPSTracking(fg, map), 10000)
})
</script>

<template>
   <div id="map" ref="mapElement" class="grow min-h-80 z-0"></div>
</template>
