<script setup>

  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';
  import { computed, onMounted, provide, ref, watch } from 'vue';
  import axios from 'axios';

  const cart = ref([])
  const isCreatingOrder = ref(false)

  const drawerOpen = ref(false)

  const totalPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
  )

  const closeDrawer = () => {
    drawerOpen.value = false
  }

  const openDrawer = () => {
    drawerOpen.value = true
  }

  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
  }

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }

  const createOrder = async () => {
    try {
      isCreatingOrder.value = true      
      const { data } = await axios.post(`https://c25052030383a4d5.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value
      })
      cart.value = []
      return data
    } catch (error) {
      console.log(err)
    } finally {
      isCreatingOrder.value = false
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

  onMounted(async() =>  {
    await fetchItems()
    await fetchFavorites()
  })

  watch(cart, () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false
    }))
  })

  provide('addToFavorite', addToFavorite)

  provide('cart', {
    cart,
    addToCart,
    removeFromCart,
    closeDrawer,
    openDrawer
  })

  provide('addToCart', addToCart)
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" @create-order="createOrder" is-creating-order="isCreatingOrder"/>
  <div class="bg-white w-4/5 m-auto h-full rounded-xl shadow-xl mt-10">
    <Header :total-price="totalPrice" @openDrawer="openDrawer" />
    <div class="p-10">
      
    </div>
  </div>
</template>

