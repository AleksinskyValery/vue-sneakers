<script setup>
import { onMounted, inject, provide, watch } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'
import InfoBlock from '@/components/InfoBlock.vue'

const favorites = inject('favorites', [])

const { cart, checkToCart, clickToCart } = inject('cart')

onMounted(async () => {
  const { data } = await axios.get('https://6dd5d0aa39a9775a.mokky.dev/favorites?_relations=items')

  favorites.value = data.map((obj) => ({
    ...obj.item,
    favoriteId: obj.id,
    isFavorite: true
  }))
  checkToCart(favorites)
})

const removeFromFavorites = async (item) => {
  try {
    item.isFavorite = false
    await axios.delete(`https://6dd5d0aa39a9775a.mokky.dev/favorites/${item.favoriteId}`)
    favorites.value = favorites.value.filter((obj) => obj.isFavorite)
  } catch (err) {
    console.log(err)
  }
}

watch(cart.value, () => {
  checkToCart(favorites)
})

provide(favorites, 'favorites')
</script>

<template>
  <div class="flex justify-between items-center flex-wrap gap-5">
    <h2 class="text-3xl font-bold">Избранное</h2>
  </div>

  <CardList
    :items="favorites"
    :on-click-favorite="removeFromFavorites"
    :on-click-add="clickToCart"
  />
  <div v-if="favorites.length === 0" class="flex justify-center items-center min-h-[50vh]">
    <InfoBlock
      title="Нет избранных"
      description="Вы ничего не добавляли в избранное"
      image-url="emoji-1.png"
    />
  </div>
</template>
