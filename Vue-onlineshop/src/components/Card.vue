<script setup>
  defineProps({
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickFavorite: Function,
    onClickAdd: Function,
    id: Number
    })
</script>

<template>
  <div>
    <div class="relative bg-white m-5 border border-slate-100 rounded-3xl p-6 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
      <img
          :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" 
          class="absolute top-3 left-3 h-7"
          @click="onClickFavorite"
      ></img>
      <img :src="imageUrl" alt="sneakers-1"></img>
      <p class="mt-2">{{ title }}</p>
      <div class="flex justify-between mt-5">
        <div ckass="flex flex-col">
          <span class="text-slate-500">Цена: </span>
          <b>{{ price }}</b>
        </div>
        <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : 'checked.svg'" alt="plus"></img>
      </div>
    </div>
  </div>
</template>