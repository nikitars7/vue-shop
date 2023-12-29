<script setup>
import CardList from '../components/cardList/CardList.vue'
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue'
import debounce from 'lodash.debounce'
const { addToCart, removeFromCart, cartItems } = inject('cart')
const items = ref([])
const filters = reactive({
  sortBy: 'title',
  order: '',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearch = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)
const addToFavotire = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true
      const { data } = await axios.post(
        `https://6581bcd53dfdd1b11c441563.mockapi.io/favorites`,
        obj
      )
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://6581bcd53dfdd1b11c441563.mockapi.io/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}
const onClickPlus = (item) => {
  const findCopy = cartItems.value.find((product) => product.id === item.id)
  if (!findCopy) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
const getParams = () => {
  const order = filters.sortBy.includes('-') ? 'desc' : 'asc'
  const sortBy = filters.sortBy.replace('-', '')
  if (filters.searchQuery) {
    return { search: filters.searchQuery, order, sortBy }
  }
  return { order, sortBy }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://6581bcd53dfdd1b11c441563.mockapi.io/favorites`
    )
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (e) {
    console.log(e.message)
  }
}
const fetchSneakers = async (params) => {
  try {
    const { data } = await axios.get(`https://6404ecfc40597b65de2d48a6.mockapi.io/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (e) {
    console.log(e.message)
  }
}
watch(filters, () => {
  const params = getParams()
  fetchSneakers(params)
})
watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
onMounted(async () => {
  cartItems.value = JSON.parse(localStorage.getItem('cart') || [])
  const params = getParams()
  await fetchSneakers(params)
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>
<template>
  <div class="flex flex-col gap-5 justify-between items-center mb-10 md:flex-row">
    <h2 class="text-3xl font-bold">Sneakers</h2>
    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        name=""
        id=""
        class="py-2 px-3 border rounded-md outline-none"
      >
        <option value="title">Name</option>
        <option value="price">Price(ASC)</option>
        <option value="-price">Price(DESC)</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
        <input
          @input="onChangeSearch"
          class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Search..."
        />
      </div>
    </div>
  </div>
  <CardList :items="items" @add-to-favorite="addToFavotire" @add-to-cart="onClickPlus" />
</template>
