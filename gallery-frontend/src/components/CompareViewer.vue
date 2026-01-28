<template>
  <div class="compare-container">
    <div id="viewer1" class="viewer"></div>
    <div id="viewer2" class="viewer"></div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import OpenSeadragon from 'openseadragon'

const props = defineProps<{
  image1Url: string
  image2Url: string
}>()

let viewer1: OpenSeadragon.Viewer
let viewer2: OpenSeadragon.Viewer
let syncing = false

onMounted(() => {
  viewer1 = OpenSeadragon({
    id: 'viewer1',
    tileSources: {
      type: 'image',
      url: props.image1Url
    },
    showNavigator: true
  })

  viewer2 = OpenSeadragon({
    id: 'viewer2',
    tileSources: {
      type: 'image',
      url: props.image2Url
    },
    showNavigator: true
  })

  // Sync zoom and pan between viewers
  viewer1.addHandler('zoom', (event) => {
    if (!syncing) {
      syncing = true
      viewer2.viewport.zoomTo(event.zoom, event.refPoint, false)
      syncing = false
    }
  })

  viewer1.addHandler('pan', (event) => {
    if (!syncing) {
      syncing = true
      viewer2.viewport.panTo(event.center, false)
      syncing = false
    }
  })

  viewer2.addHandler('zoom', (event) => {
    if (!syncing) {
      syncing = true
      viewer1.viewport.zoomTo(event.zoom, event.refPoint, false)
      syncing = false
    }
  })

  viewer2.addHandler('pan', (event) => {
    if (!syncing) {
      syncing = true
      viewer1.viewport.panTo(event.center, false)
      syncing = false
    }
  })
})

onUnmounted(() => {
  if (viewer1) viewer1.destroy()
  if (viewer2) viewer2.destroy()
})
</script>

<style scoped>
.compare-container {
  display: flex;
  height: 600px;
}
.viewer {
  flex: 1;
  border: 1px solid #ccc;
}
</style>