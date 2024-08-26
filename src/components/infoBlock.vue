<script setup>
defineProps({
  imgUrl: String,
  title: String,
  description: String
})
</script>

<template>
  <div class="flex flex-col items-center flex-1 justify-center">
    <img :src="imgUrl" alt="Info image" class="w-[120px] h-[120px]" />
    <h2 class="text-2xl font-bold text-slate-700 mt-2">{{ title }}</h2>
    <p class="text-lg text-slate-500 mt-2 leading-6 w-72">{{ description }}</p>
  </div>
</template>
