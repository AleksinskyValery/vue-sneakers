<script setup>
defineProps({
  id: Number,
  title: String,
  price: Number,
  imageUrl: String,
  removeFromCart: Function
})
</script>

<template>
  <div class="flex justify-between itens-center gap-10 border border-slate-200 p-4 rounded-xl">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />
    <div class="flex flex-col">
      <p class="mb-2">{{ title }}</p>

      <div class="flex justify-between">
        <span class="font-bold">{{ price }} руб.</span>
        <button
          @click="removeFromCart"
          type="button"
          aria-label="Удалить из корзины"
          class="w-8 h-8 bg-[url(/close.svg)] bg-cover bg-no-repeat opacity-60 hover:opacity-100 transition"
          title="Удалить из корзины"
        ></button>
      </div>
    </div>
  </div>
</template>
