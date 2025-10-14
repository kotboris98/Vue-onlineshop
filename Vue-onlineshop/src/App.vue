<script setup>

  import Header from './components/Header.vue';
  import CardList from './components/CardList.vue';
  import Drawer from './components/Drawer.vue';
  import { onMounted, reactive, ref, watch } from 'vue';
  import axios from 'axios';

  const items = ref([])
  const searchQuery = ref('')

  const filter = reactive({
    searchQuery: ''
  })

  const onChangeSearchInput = (event) => {
    filter.searchQuery = event.target.value
  }

  const searchItems = async () => {
    try {

    }
    const params = {
      searchQuery: filter.searchQuery
    }
  }

  onMounted(async() => {
    try {
      const { data } = await axios.get(`https://c25052030383a4d5.mokky.dev/items`, {
        params
      })
      items.value = data
  } catch (err) {
      console.log(err)
  }
  })
  
  watch()
</script>

<template>
  <!--Drawer /> -->
  <div class="bg-white w-4/5 m-auto h-full rounded-xl shadow-xl mt-10">
    <Header />
    <div class="p-10">
      <div class=" flex justify-between">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <select>
          <option>По названию</option>
          <option>По цене (дешевые)</option>
          <option>По цене (дорогие)</option>
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
      <CardList :items="items" />
    </div>
  </div>
</template>

