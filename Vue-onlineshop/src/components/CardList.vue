<script setup>
    import Card from './Card.vue';

    defineProps({
        items: Array,
    })

    const emit = defineEmits(['addToFavorite'])
</script>

<template>
    <div class="grid grid-cols-4 gap-2">
        <Card 
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :title="item.title" 
        :image-url="item.imageUrl"
        :price="item.price"
        :onClickAdd="() => emit('addToCart', item)"
        :onClickFavorite="() => emit('addToFavorite', item)"
        :isFavorite="item.isFavorite"
        :isAdded="item.isAdded" />
      </div>
</template>