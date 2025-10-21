<script setup>
    import { inject } from 'vue';
    import DrawerItems from './DrawerItems.vue';
    import DrawerHeader from './DrawerHeader.vue';

    const { cart, removeFromCart } = inject('cart')

    const emit = defineEmits(['createOrder'])

    defineProps({
        totalPrice: Number,
        isCreatingOrder: Boolean
    })

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
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHeader />
        <DrawerItems 
            v-for="item in cart" 
            :key="item.id" 
            :title="item.title"
            :price="item.price"
            :image-url="item.imageUrl"
            @on-click-remove="() => removeFromCart(item)" />
        <div class="flex gap-2 mb-6 mt-6">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }}</b>
        </div>
        <button
            disabled="" 
            class="bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600"
            @click="() => emit('createOrder')">
            Оформить заказ
        </button>
    </div>
</template>