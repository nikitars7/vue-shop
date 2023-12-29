<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
const emit = defineEmits(['onClickRemove'])
</script>
<template>
  <div class="border border-slate-200 p-4 rounded-xl flex gap-5 items-center">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col flex-auto">
      <p>{{ title }}</p>
      <div class="flex justify-between">
        <b>{{ price }}$</b>
        <img
          @click="emit('onClickRemove')"
          src="/close.svg"
          alt="close"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
