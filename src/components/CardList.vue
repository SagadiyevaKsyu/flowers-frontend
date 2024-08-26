<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  addToCart: Function,
  addToFavorite: Function
})
</script>

<template>
  <div class="grid gap-5 mt-12" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
      :onClickFavorite="() => addToFavorite(item)"
      :onClickAdd="() => addToCart(item)"
    />
  </div>
</template>
<style scoped>
div {
  grid-template-columns: repeat(auto-fill, minmax(min(20rem, 100%), 1fr));
}
</style>
