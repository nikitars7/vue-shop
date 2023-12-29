<script setup>
import { ref, watch, provide, computed } from 'vue'
import Header from './components/header/Header.vue'
import Drawer from './components/drawer/Drawer.vue'
/* Drawer*/
const cartItems = ref([])

const isDrawerOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => item.price + acc, 0))

const openDrawer = () => {
  isDrawerOpen.value = true
}
const closeDrawer = () => {
  isDrawerOpen.value = false
}

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}
/* Drawer*/

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)
provide('cart', {
  cartItems,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
  totalPrice
})
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer v-if="isDrawerOpen" />
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
