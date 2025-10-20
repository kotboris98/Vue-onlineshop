<script setup>
    import { inject } from 'vue';

    const { closeDrawer } = inject('cart')
</script>

<template>
    <div class="flex items-center gap-40 mb-5">
        <h2 class="text-2xl font-bold">Корзина</h2>
        <img @click="closeDrawer" src="/close-red-icon.svg" class="w-10 opacity-70 cursor-pointer"></img>
    </div>
</template>
