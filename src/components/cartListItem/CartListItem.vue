<script setup>
import { inject } from 'vue'
import CartItem from '../cartItem/CartItem.vue'
const { cartItems, removeFromCart } = inject('cart')
</script>
<template>
  <div v-auto-animate class="flex flex-col gap-5 flex-1">
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>
