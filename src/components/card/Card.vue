<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})
</script>
<template>
  <div
    class="flex flex-col relative bg-white border border-slate-100 rounded-3xl p-8 hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      v-if="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="favorite1"
      class="absolute top-8 left-8 cursor-pointer"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="Sneaker" class="flex-1" />
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Price:</span>
        <b>{{ price }}$</b>
      </div>
      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="plus"
        class="cursor-pointer"
      />
    </div>
  </div>
</template>
