<script setup>
    import { inject } from 'vue';
    import Card from './Card.vue';

    defineProps({
        items: Array,
        addToFavorite: Function
    })

    const onClickAdd = () => {
        alert('Добавить!')
    }

    const addToFavorite = inject('addToFavorite')
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
        :onClickAdd="onClickAdd"
        :onClickFavorite="() => addToFavorite(item)"
        :isFavorite="item.isFavorite" />
      </div>
</template>