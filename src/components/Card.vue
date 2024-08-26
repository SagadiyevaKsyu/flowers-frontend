<script setup>
defineProps({
  title: String,
  price: Number,
  imageUrl: String,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    id="main"
    class="relative border overflow-hidden rounded-3xl p-8 bg-white cursor-pointer hover:shadow-xl hover:-translate-y-2 duration-300"
  >
    <img
      @click="onClickFavorite"
      :src="isFavorite ? '/like-2.png' : '/like-1.png'"
      alt="Like"
      class="absolute w-8 hover:scale-110 duration-300"
    />
    <img :src="imageUrl" alt="Flower" id="ImgCard" class="img-scale pointer-events-none" />
    <p class="text-lg mt-8">{{ title }}</p>
    <div class="flex items-center mt-5 justify-between">
      <div class="flex flex-col">
        <span class="text-slate-300 text-lg">Цена (за 1 шт.)</span>
        <b class="text-lg">{{ price }} руб.</b>
      </div>
      <img
        @click="onClickAdd"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Add to buy"
        class="w-8"
      />
    </div>
  </div>
</template>

<style scoped>
#ImgCard {
  transition: 200ms;
}

div#main:hover #ImgCard {
  transform: scale(1.3) rotate(-6deg);
  transition-duration: 300ms;
}
</style>
