<script setup>
    import CardList from '@/components/CardList.vue';
    import axios from 'axios';
    import { onMounted, ref } from 'vue';

    const favorites = ref([])

    const fetchFavorites = async () => {
        try {
           const { data } = await axios.get('https://c25052030383a4d5.mokky.dev/favorites') 
           const itemsRes = await axios.get('https://c25052030383a4d5.mokky.dev/items')
           favorites.value = itemsRes.data.filter(item => data.some(fav => fav.parentId === item.id))
        } catch (err) {
           console.log(err)
        }
    }

    onMounted(fetchFavorites)
</script>

<template>
    <div class="p-10">
        <div class="flex justify-between items-center mb-8">
            <h2 class="text-3xl font-bold">Мои закладки</h2>
            <router-link to="/" class="text-blue-500 hover:underline">← Назад</router-link>
        </div>

        <CardList :items="favorites" />
    </div>
</template>