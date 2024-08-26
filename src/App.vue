<script setup>
import { onMounted, ref, watch, provide } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'
import InfoBlock from './components/infoBlock.vue'
import CardList from './components/CardList.vue'

// все товары
const items = ref([])

// для товаров, добавленных в корзину
const cartItems = ref([])

function removeFromCart(item) {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

function onClickAddToCart(item) {
  if (!item.isAdded) {
    cartItems.value.push(item)
    item.isAdded = true
  } else {
    removeFromCart(item)
  }
}

// для товаров, добавленных в избранное
const favoriteItems = ref([])

function onClickAddToFavorite(item) {
  if (!item.isFavorite) {
    favoriteItems.value.push(item)
    item.isFavorite = true
    console.log(favoriteItems.value)
  } else {
    favoriteItems.value.splice(favoriteItems.value.indexOf(item), 1)
    item.isFavorite = false
  }
}
async function getItemsFromServer(url) {
  let resp = await fetch(url) // запрашиваем данные json с сервера (данные карточек)
  items.value = await resp.json() // сохраняем данные в нужную переменную
}

const sortBy = ref('')
const searchQuery = ref('')

const onChangeSelect = (event) => {
  sortBy.value = event.target.value
}
const onChangeInput = (event) => {
  searchQuery.value = event.target.value
}

// для отображения корзины по клику в шапке и скрытия корзины
const drawVisible = ref(false)
function drawOpenFunc() {
  drawVisible.value = true
}
function drawCloseFunc() {
  drawVisible.value = false
}
// для отображения избранного по клику в шапке и скрытия
const favoriteVisible = ref(false)
function favVisibleFun() {
  favoriteVisible.value = true
}
function favClose() {
  favoriteVisible.value = false
}

onMounted(async () => {
  const localCartItems = localStorage.getItem('cartItemsLoc')
  const localFavItems = localStorage.getItem('favoriteItemsLoc')

  cartItems.value = localCartItems ? JSON.parse(localCartItems) : []
  favoriteItems.value = localFavItems ? JSON.parse(localFavItems) : []

  await getItemsFromServer('https://bb67c475ee484653.mokky.dev/items')

  // перебор полученного массива, проверяем есть ли в нем добавленные товары
  // или лайкнутые и меняем isAdded и isFavorite
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cart) => cart.id === item.id),
    isFavorite: favoriteItems.value.some((fav) => fav.id === item.id)
  }))
})

// отслеживаем изменение выбора в выпадающием списке,в поле ввода для поиска
// и запрашиваем данные с фильтрацией ?title=* и &sortBy=
watch([sortBy, searchQuery], 
  getItemsFromServer(`https://bb67c475ee484653.mokky.dev/items?title=*${searchQuery.value}&sortBy=${sortBy.value}`)
)

// стоимость корзины
const summ = ref(0)

watch(
  cartItems,
  async () => {
    summ.value = cartItems.value.reduce((acc, cur) => acc + cur.price, 0)
    localStorage.setItem('cartItemsLoc', JSON.stringify(cartItems.value))
  },
  { deep: true }
)

watch(
  favoriteItems,
  async () => {
    localStorage.setItem('favoriteItemsLoc', JSON.stringify(favoriteItems.value))
  },
  { deep: true }
)

provide('sumPrice', { summ })
provide('cartItems', { cartItems })
provide('favoriteItems', { favoriteItems })
provide('remove', { removeFromCart })
provide('favVisible', { favoriteVisible })
</script>

<template>
  <Drawer v-if="drawVisible" :drawClose="drawCloseFunc" />

  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14 mb-14">
    <!-- шапка -->
    <Header :drawOpen="drawOpenFunc" :favVis="favVisibleFun" :favClose="favClose" />

    <!-- тело сайта -->
    <div class="p-10 min-h-[75vh] flex flex-col">
      <!-- блок, появляющийся для раздела избранное -->
      <div
        @click="favClose"
        v-if="favoriteVisible"
        class="text-slate-500 hover:text-slate-900 duration-300 cursor-pointer flex gap-2 -mt-3 mb-3"
      >
        <img src="/public/arrow-right.svg" alt="Undo" class="rotate-180" />
        <p class="">Вернуться в каталог</p>
      </div>

      <div class="flex justify-between items-center">
        <h1 class="text-3xl font-bold">{{ favoriteVisible ? 'Избранное' : 'Каталог' }}</h1>
        <!-- поиск и фильтры -->
        <div v-if="!favoriteVisible" class="flex gap-5">
          <select
            @change="onChangeSelect"
            class="border outline-none rounded-2xl py-2 px-4 text-base text-slate-500 focus:border-purple-300"
          >
            <option value="name">По названию</option>
            <option value="price">По цене (по возрастанию)</option>
            <option value="-price">По цене (по убыванию)</option>
          </select>

          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute top-3 left-3.5" />
            <input
              @input="onChangeInput"
              type="text"
              placeholder="Поиск..."
              class="text-base border rounded-2xl pl-10 pr-4 py-2 outline-none focus:border-purple-300"
            />
          </div>
        </div>
      </div>

      <!-- блок, появляющийся, когда ничего не найдено при поиске -->
      <InfoBlock
        v-if="items.length === 0"
        img-url="/dead_flower.png"
        title="Ничего не найдено"
        description="Попробуйте ввести другой запрос"
      />

      <!-- блок, появляющийся в избранном, когда в нем пусто -->
      <InfoBlock
        v-if="favoriteItems.length === 0 && favoriteVisible"
        img-url="/dead_flower.png"
        title="В избранном пусто :("
        description="Добавьте хотя бы один цветочек"
      />

      <!-- избрынные товары -->
      <CardList
        v-if="favoriteVisible"
        :items="favoriteItems"
        :addToCart="onClickAddToCart"
        :addToFavorite="onClickAddToFavorite"
      />

      <!-- карточки товаров -->
      <CardList
        v-else
        v-if="items"
        :items="items"
        :addToCart="onClickAddToCart"
        :addToFavorite="onClickAddToFavorite"
      />
    </div>
  </div>
</template>

<style>
body {
  background-color: #9b806126;
}
</style>
