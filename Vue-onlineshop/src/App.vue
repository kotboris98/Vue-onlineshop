<script setup>
  import Header from './components/Header.vue';
  import CardList from './components/CardList.vue';
  import Drawer from './components/Drawer.vue';
</script>

<template>
  <!--Drawer /> -->
  <div class="bg-white w-4/5 m-auto h-screen rounded-xl shadow-xl mt-10">
    <Header />
    <div class="p-10">
      <div class=" flex justify-between">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="relative">
          <input class="border-2 border-rounded-md  py-2 pl-10 pr-4" type="text" placeholder="Поиск..."></input>
        </div>
      </div>
      <CardList />
    </div>
  </div>
</template>

