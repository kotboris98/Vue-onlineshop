<script setup>

  import Header from './components/Header.vue';
  import CardList from './components/CardList.vue';
  import Drawer from './components/Drawer.vue';
  import { onMounted, provide, reactive, ref, watch } from 'vue';
  import axios from 'axios';

  const items = ref([])

  const drawerOpen = ref(false)

  const closeDrawer = () => {
    drawerOpen.value = false
  }

  const openDrawer = () => {
    drawerOpen.value = true
  }

  const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
  })

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
  }

  const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
  }

  const fetchFavorites = async () => {
    try {
      const { data: favorites } = await axios.get(`https://c25052030383a4d5.mokky.dev/favorites`)
      items.value = items.value.map((item) => {
        const favorite = favorites.find((favorite) => favorite.parentId === item.id)
        if (!favorite) {
          return item
        }
        return {
          ...item,
          isFavorite: true,
          favoriteId: favorite.id
        }
      })
  } catch (err) {
      console.log(err)
  }
  }

  const addToFavorite = async(item) => {
    try {
      if (!item.isFavorite) {
        const obj = {
          parentId: item.id
        }
        const { data } = await axios.post(`https://c25052030383a4d5.mokky.dev/favorites`, obj)
        item.isFavorite = true
        item.favoriteId = data.id
      } else {
        await axios.delete(`https://c25052030383a4d5.mokky.dev/favorites/${item.favoriteId}`)
        item.isFavorite = false
        item.favoriteId = null
      }
    } catch (err) {
      console.log(err)
    }
    }

  const fetchItems = async() => {
    try {
      const params = {
        sortBy: filters.sortBy
      }
      if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
      }

      const { data } = await axios.get(`https://c25052030383a4d5.mokky.dev/items`, {
        params
      })
      items.value = data.map(obj => ({
        ...obj,
        isFavorite: false,
        favoriteId: null,
        isAdded: false
      }))
  } catch (err) {
      console.log(err)
  }
  }

  onMounted(async() =>  {
    await fetchItems()
    await fetchFavorites()
  })
  watch(filters, fetchItems)

  provide('addToFavorite', addToFavorite)

  provide('cartActions', {
    closeDrawer,
    openDrawer
  })
</script>

<template>
  <Drawer v-if="drawerOpen" />
  <div class="bg-white w-4/5 m-auto h-full rounded-xl shadow-xl mt-10">
    <Header @openDrawer="openDrawer" />
    <div class="p-10">
      <div class=" flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <input 
              @input="onChangeSearchInput"
              class="border-2 border-rounded-md  py-2 pl-10 pr-4" 
              type="text" 
              placeholder="Поиск...">
            </input>
          </div>
        </div>
      </div>
      <CardList :items="items" @addToFavorite="addToFavorite" />
    </div>
  </div>
</template>

