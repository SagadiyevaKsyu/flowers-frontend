<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cartItems } = inject('cartItems')
const { removeFromCart } = inject('remove')
</script>

<template>
  <div class="flex flex-col gap-4 flex-1" v-auto-animate>
    <CartItem
      v-for="cartItem in cartItems"
      :key="cartItem.id"
      :title="cartItem.title"
      :price="cartItem.price"
      :imgUrl="cartItem.imageUrl"
      :onClickRemove="() => removeFromCart(cartItem)"
    />
  </div>
</template>
