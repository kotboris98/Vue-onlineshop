<script setup>

  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';
  import { computed, provide, ref, watch } from 'vue';
  import axios from 'axios';
  import Home from './pages/Home.vue';

  const cart = ref([])

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
      <router-view></router-view>
    </div>
  </div>
</template>

