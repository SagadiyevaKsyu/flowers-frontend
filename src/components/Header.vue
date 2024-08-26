+<script setup>
import { inject } from 'vue'

defineProps({
  drawOpen: Function,
  favVis: Function,
  favClose: Function
})

const { summ } = inject('sumPrice')
const { favVisible } = inject('favVisible')
</script>

<template>
  <div class="flex justify-between border-b px-10 py-8 sticky top-0 bg-white z-20 rounded-t-xl">
    <div class="flex items-center">
      <img @click="favClose" src="/logo.png" alt="Logo" class="w-10 cursor-pointer" />
      <div class="ml-4">
        <h2 class="font-bold">FLOWER APP</h2>
        <p class="text-slate-500">Магазин безупречных цветов</p>
      </div>
    </div>

    <!-- меню -->
    <ul class="flex gap-8 items-center">
      <!-- корзина -->
      <li @click="drawOpen" class="flex gap-2 cursor-pointer">
        <img src="/cart.svg" alt="Cart" class="w-5" />
        <p class="text-slate-500 hover:text-slate-950 font-bold">{{ summ }} руб.</p>
      </li>
      <!-- избранное -->
      <li @click="favVis" class="flex gap-2 cursor-pointer active:text-bold">
        <img src="/heart.svg" alt="Favorite" class="w-5" />
        <p class="text-slate-500 hover:text-slate-950">Избранное</p>
      </li>
      
    </ul>
  </div>
</template>
