<script setup>
  import MyHelloVue from "./components/MyHello.vue";
</script>

<template>
  <div class="mb-40">
  <MyHelloVue title="Puma sneakers" :price="15000" :isAdded="true">
    <button>Add</button>
  </MyHelloVue>
  </div>
</template>

