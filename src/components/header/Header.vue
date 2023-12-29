<script setup>
const emit = defineEmits(['openDrawer'])
defineProps({
  totalPrice: Number
})
</script>
<template>
  <header class="flex items-center justify-between border-b border-slate-300 px-10 py-8">
    <router-link to="/">
      <div class="flex gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">The best sneakers shop</p>
        </div>
      </div>
    </router-link>
    <ul class="flex gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex gap-3 items-center text-slate-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="Cart" />
        <span>{{ totalPrice }}$</span>
      </li>
      <router-link to="/favorites">
        <li class="flex gap-3 items-center text-slate-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="Favorite" />
          <span>Favorites</span>
        </li>
      </router-link>
      <li class="flex gap-3 items-center text-slate-500 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <span>Profile</span>
      </li>
    </ul>
  </header>
</template>
