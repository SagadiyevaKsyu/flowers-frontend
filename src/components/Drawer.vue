<script setup>
import { inject } from 'vue'
import { vOnClickOutside } from '@vueuse/components'
import CartItemList from './CartItemList.vue'
import infoBlock from './infoBlock.vue'

const { summ } = inject('sumPrice')

defineProps({
  drawClose: Function
})

function createOrder(){
  
}
</script>

<template>
  <!-- блок для затемнения при отображении корзины -->
  <div class="fixed top-0 left-0 h-full w-full bg-black opacity-60 z-30"></div>
  <!-- сама корзина -->
  <div
    v-on-click-outside="drawClose"
    class="fixed top-0 right-0 w-[500px] bg-white h-full z-40 p-10 flex flex-col"
  >
    <!-- шапка корзины -->
    <div class="flex items-center mb-8">
      <svg
        @click="drawClose"
        class="cursor-pointer rotate-180 mr-5 opacity-40 hover:opacity-100 duration-300"
        width="12"
        height="19"
        viewBox="0 0 7 12"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M1 0.999999L6 6L1 11"
          stroke="black"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>

      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>

    <!-- блок, появляющийся, когда товаров в корзин нет -->
    <infoBlock
      v-if="summ == 0"
      title="Корзина пустая :("
      description="Добавьте хотя бы один цветочек в корзину, чтобы оформить заказ"
      img-url="/package-icon.png"
    />

    <!-- товары корзины -->
    <CartItemList v-if="summ > 0" />

    <!-- нижняя часть корзины (итого) -->
    <div v-if="summ > 0" class="my-4">
      <div class="flex">
        <span class="text-lg">Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b class="text-lg">{{ summ }} руб.</b>
      </div>
      <button
      @click="createOrder"
        :disabled="summ == 0 ? '' : disabled"
        class="cursor-pointer bg-orange-400 hover:bg-orange-500 duration-300 disabled:bg-slate-300 active:bg-orange-600 py-4 w-full rounded-3xl mt-5 text-white text-lg"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
