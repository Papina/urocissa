<template>
  <div class="slider-container">
    <div class="slider-wrapper" ref="sliderWrapper" @mousedown="startDrag" @touchstart="startDrag">
      <div class="image-wrapper">
        <img :src="image2Url" alt="Image 2" class="background-image" />
        <img
          :src="image1Url"
          alt="Image 1"
          class="foreground-image"
          :style="{ clipPath: `inset(0 ${100 - position}% 0 0)` }"
        />
        <div class="slider" :style="{ left: `${position}%` }">
          <div class="slider-handle"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps<{
  image1Url: string
  image2Url: string
}>()

const position = ref(50)
const isDragging = ref(false)
const sliderWrapper = ref<HTMLElement>()

const startDrag = (e: MouseEvent | TouchEvent) => {
  isDragging.value = true
  updatePosition(e)
  e.preventDefault()
}

const updatePosition = (e: MouseEvent | TouchEvent) => {
  if (!isDragging.value || !sliderWrapper.value) return

  const rect = sliderWrapper.value.getBoundingClientRect()
  const clientX = 'clientX' in e ? e.clientX : e.touches?.[0]?.clientX || 0
  const x = clientX - rect.left
  position.value = Math.max(0, Math.min(100, (x / rect.width) * 100))
}

const stopDrag = () => {
  isDragging.value = false
}

onMounted(() => {
  document.addEventListener('mousemove', updatePosition)
  document.addEventListener('touchmove', updatePosition)
  document.addEventListener('mouseup', stopDrag)
  document.addEventListener('touchend', stopDrag)
})

onUnmounted(() => {
  document.removeEventListener('mousemove', updatePosition)
  document.removeEventListener('touchmove', updatePosition)
  document.removeEventListener('mouseup', stopDrag)
  document.removeEventListener('touchend', stopDrag)
})
</script>

<style scoped>
.slider-container {
  position: relative;
  width: 100%;
  height: 600px;
}

.slider-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  cursor: ew-resize;
  overflow: hidden;
}

.image-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
}

.background-image,
.foreground-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.slider {
  position: absolute;
  top: 0;
  width: 2px;
  height: 100%;
  background-color: white;
  cursor: ew-resize;
  z-index: 10;
}

.slider-handle {
  position: absolute;
  top: 50%;
  left: -10px;
  width: 20px;
  height: 20px;
  background-color: white;
  border-radius: 50%;
  transform: translateY(-50%);
  cursor: ew-resize;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
}
</style>