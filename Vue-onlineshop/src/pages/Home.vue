<script setup>
    import { inject, reactive, watch } from 'vue';
    import CardList from '../components/CardList.vue';
    import axios from 'axios'; 
    import { ref } from 'process';

    const { addToCart, removeFromCart } = inject('cart')

    const items = ref([])

    const filters = reactive({
        sortBy: 'title',
        searchQuery: ''
    })

    const onClickAddPlus = (item) => {
        if (!item.isAdded) {
            addToCart(item)
        } else {
            removeFromCart(item)
        }
        console.log(cart)
    }

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

    watch(filters, fetchItems)
</script>

<template>
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
      <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
</template>