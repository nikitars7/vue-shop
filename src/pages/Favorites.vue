<script setup>
import { ref, onMounted } from 'vue'
import CardList from '../components/cardList/CardList.vue'
import axios from 'axios'
const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://6581bcd53dfdd1b11c441563.mockapi.io/favorites')
    favorites.value = data.map((obj) => obj.item)
  } catch (e) {
    console.log(e.message)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-10">Favorites</h2>
  <CardList :items="favorites" is-favorite />
</template>
