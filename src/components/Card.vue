<script setup>
defineProps({
  imgUrl: String,
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
    class="relative bg-white border border-slate-100 rounded-3xl p-8 hover:-translate-y-2 hover:shadow-xl transition"
  >
    <button
      type="button"
      :aria-label="isFavorite ? 'Убрать из избранного' : 'Добавить в избранное'"
      class="absolute top-8 left-8 w-8 h-8 bg-cover bg-no-repeat hover:scale-105 transition"
      :class="isFavorite ? 'bg-[url(/like-2.svg)]' : 'bg-[url(/like-1.svg)]'"
      @click="onClickFavorite"
      :title="isFavorite ? 'Убрать из избранного' : 'Добавить в избранное'"
    ></button>
    <div class="max-w-[266px] mx-h-[224px]">
      <img class="object-cover w-full h-auto" :src="imgUrl" alt="{{ title }}" />
    </div>

    <h3 class="mt-2">{{ title }}</h3>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <span class="font-bold">{{ price.toLocaleString('ru-RU') }} ₽</span>
      </div>
      <button
        class="w-8 h-8 bg-cover bg-no-repeat hover:scale-105 transition"
        :class="isAdded ? 'bg-[url(/checked.svg)]' : 'bg-[url(/plus.svg)]'"
        :aria-label="isAdded ? 'Убрать из корзины' : 'Добавить в корзину'"
        @click="onClickAdd"
        :title="isAdded ? 'Убрать из корзины' : 'Добавить в корзину'"
      ></button>
    </div>
  </div>
</template>
