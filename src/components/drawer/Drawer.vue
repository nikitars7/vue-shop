<script setup>
import DrawerHead from './DrawerHead.vue'
import { ref, watch, computed } from 'vue'
import CartListItem from '../cartListItem/CartListItem.vue'
import InfoBlock from '../infoblock/InfoBlock.vue'
import axios from 'axios'
import { inject } from 'vue'
const { totalPrice, cartItems } = inject('cart')
const isCreatingOrder = ref(false)
const isOrder = ref(null)

const cartIsEmpty = computed(() => cartItems.value.length === 0)
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)
const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://6581bcd53dfdd1b11c441563.mockapi.io/orders`, {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })
    cartItems.value = []
    isOrder.value = data.id
    return data
  } catch (e) {
    console.log(e.message)
  } finally {
    isCreatingOrder.value = false
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70">1</div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-10 overflow-auto flex flex-col gap-5">
    <DrawerHead />
    <div v-if="!totalPrice || isOrder" class="flex h-full items-center justify-center">
      <InfoBlock
        v-if="!totalPrice && isOrder"
        title="Order is completed"
        description="Your order #18 will be delivered ASAP"
        image-url="/order-success-icon.png"
      />
      <InfoBlock
        v-else-if="!totalPrice"
        title="Cart is empty"
        description="You need to add at least 1 item"
        image-url="/package-icon.png"
      />
    </div>
    <CartListItem v-if="totalPrice" />

    <div v-if="totalPrice" class="flex flex-col gap-4 mb-6">
      <div class="flex gap-2">
        <span>Summary:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }}$</b>
      </div>
      <button
        @click="createOrder"
        :disabled="cartButtonDisabled"
        class="mt-7 bg-lime-500 w-full cursor-pointer disabled:bg-slate-400 rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700"
      >
        Checkout
      </button>
    </div>
  </div>
</template>
