<script setup>
    import { inject } from 'vue';
    import DrawerItems from './DrawerItems.vue';

    const { closeDrawer } = inject('cart')
    const { cart } = inject('cart')
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <div class="flex items-center gap-40 mb-5">
            <h2 class="text-2xl font-bold">Корзина</h2>
            <img @click="closeDrawer" src="/close-red-icon.svg" class="w-10 opacity-70 cursor-pointer"></img>
        </div>
        <DrawerItems 
            v-for="item in cart" 
            :key="item.id" 
            :title="item.title"
            :price="item.price"
            :image-url="item.imageUrl" />
        <div class="flex gap-2 mb-6 mt-6">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>1000$</b>
        </div>
        <button class="bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600">Оформить заказ</button>
    </div>
</template>