<script setup>
    import Card from './Card.vue';

    const onClickAdd = () => {
        alert('Добавить!')
    }
</script>

<template>
    <div class="grid grid-cols-4 gap-2">
        <Card 
        title="Кроссовки Nike" 
        image-url="/sneakers-1.jpg"
        :price="1000"
        :is-added="false"
        :is-favourite="false"
        :onClickAdd="onClickAdd" />
      </div>
</template>