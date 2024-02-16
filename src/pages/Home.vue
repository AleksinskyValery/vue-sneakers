<script setup>
import { onMounted, reactive, watch, inject } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import CardList from '../components/CardList.vue'
import Sorted from '../components/Sorted.vue'

const { clickToCart, checkToCart } = inject('cart')

const { cart } = inject('cart')

const addToFavorites = inject('addToFavorites')

const items = inject('items')

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 1000)

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://6dd5d0aa39a9775a.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `**${filters.searchQuery}**`
    }
    const { data } = await axios.get('https://6dd5d0aa39a9775a.mokky.dev/items', { params })
    items.value = data.map((obj) => ({
      ...obj,
      favoriteId: null,
      isFavorite: false,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
  checkToCart(items)
})

watch(cart.value, () => {
  checkToCart(items)
})

watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center flex-wrap gap-5">
    <h2 class="text-3xl font-bold w-full">Все кроссовки</h2>
    <Sorted :on-change-select="onChangeSelect" :on-change-search-input="onChangeSearchInput" />
  </div>
  <CardList :items="items" :on-click-favorite="addToFavorites" :on-click-add="clickToCart" />
</template>
