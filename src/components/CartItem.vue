<script setup>
defineProps({
  title: String,
  price: Number,
  imgUrl: String,
  onClickRemove: Function
})
</script>

<template>
  <div class="flex relative border border-slate-200 p-5 rounded-2xl items-center">
    <img :src="imgUrl" class="h-10 mr-3" />
    <div>
      <p>{{ title }}</p>
      <b>{{ price }} руб.</b>
    </div>

    <img
      @click="onClickRemove"
      src="/delete.svg"
      class="absolute cursor-pointer transition bottom-5 right-5 opacity-60 hover:opacity-100"
    />
  </div>
</template>
