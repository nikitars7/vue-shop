<script setup>
import Card from '../card/Card.vue'
defineProps({
  items: Array,
  isFavorite: Boolean
})
const emit = defineEmits(['addToFavorite', 'addToCart'])
const onClickAdd = () => {
  alert('Added')
}
</script>

<template>
  <div v-auto-animate class="cardlist">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :is-added="item.isAdded"
      :is-favorite="item.isFavorite"
      :onClickAdd="isFavorite ? null : () => emit('addToCart', item)"
      :onClickFavorite="isFavorite ? null : () => emit('addToFavorite', item)"
    />
  </div>
</template>

<style scoped>
.cardlist {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 40px;
}
</style>
