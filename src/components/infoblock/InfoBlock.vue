<script setup>
defineProps({
  imageUrl: String,
  title: String,
  description: String
})
</script>
<template>
  <div class="flex flex-col items-center text-center gap-3">
    <img height="120" width="120" :src="imageUrl" :alt="title" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
