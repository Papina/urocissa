<template>
  <PageTemplate>
    <template #content>
      <div class="compare-page">
        <h1>Compare and Zoom</h1>
        <p>Select two images to compare.</p>
        <!-- TODO: Implement image selection and comparison -->
      </div>
    </template>
  </PageTemplate>
</template>

<script setup lang="ts">
import PageTemplate from './PageLayout/PageTemplate.vue'
</script>

<style scoped>
.compare-page {
  padding: 20px;
}
</style>