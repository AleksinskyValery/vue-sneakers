<script setup>
import axios from 'axios'
import { onMounted, ref, inject, watch } from 'vue'

const orders = ref([])

const { cart } = inject('cart')

const fetchOrders = async () => {
  const { data } = await axios.get('https://6dd5d0aa39a9775a.mokky.dev/orders')
  orders.value = data
}

onMounted(async () => {
  await fetchOrders()
})
watch(cart, () => {
  fetchOrders()
})
</script>

<template>
  <div class="flex justify-between items-center flex-wrap gap-5 mb-6">
    <h1 class="text-3xl font-bold w-full">Мои заказы</h1>
    <p class="text-slate-600">Всего заказов: {{ orders.length }}</p>
  </div>
  <ul>
    <li v-for="order in orders" :key="order.id" class="border border-slate-300 p-4 rounded-lg mb-4">
      <h2 class="text-xl font-bold w-full">Заказ №{{ order.id }}</h2>
      <div class="flex gap-4 mb-2">
        <span>Время заказа:</span>
        <time :datetime="order.createdAt">
          {{ new Date(order.createdAt).toLocaleString() }}
        </time>
      </div>
      <div class="flex flex-wrap gap-2 mb-5">
        <div v-for="item in order.items" :key="item.id">
          <img
            class="w-32 h-32 object-contain"
            :src="item.imageUrl"
            :alt="item.title"
            :title="item.title"
          />
          <span>{{ item.price.toLocaleString('ru-RU') }} ₽</span>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <span class="text-slate-600">Всего товаров: {{ order.items.length }}</span>
        <span class="text-slate-600">Итого: {{ order.totalPrice.toLocaleString('ru-RU') }} ₽</span>
      </div>
    </li>
  </ul>
</template>
