<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader'
import { onMounted, ref } from 'vue'

const loader = new Loader({
  apiKey: '<your-API-key-here>',
  libraries: ['places']
})

const map = ref<google.maps.Map>()
const mapContainer = ref<HTMLDivElement>()
const markers = ref<google.maps.Marker[]>([])

onMounted(async () => {
  await loader.load()

  map.value = new google.maps.Map(mapContainer.value as HTMLDivElement, {
    zoom: 4,
    center: { lat: 40, lng: -100 }
  })
})

const clearMarkers = () => {
  markers.value.forEach((marker) => {
    marker.setMap(null)
  })
  markers.value = []
}

const addMarkers = () => {
  const newMarkers = [
    { lat: 37.535916, lng: -96.524573 },
    { lat: 46.021019, lng: -117.147974 },
    { lat: 40.662888, lng: -81.018198 }
  ]

  newMarkers.forEach((marker) => {
    markers.value.push(new google.maps.Marker({ map: map.value, position: marker }))
  })
}
</script>

<template>
  <div>
    <button type="button" @click="addMarkers">Add markers</button>
    <button type="button" @click="clearMarkers">Clear markers</button>

    <div ref="mapContainer" style="width: 500px; height: 500px">Google Map will load here</div>
  </div>
</template>
