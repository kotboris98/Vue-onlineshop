<script setup>
    defineProps({
        title: String,
        imageUrl: String,
        price: Number,
        id: Number
    })

    const emit = defineEmits(['onClickRemove'])
</script>

<template>
    <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
            <img class="w-16 h-16" :src="imageUrl"></img>
            <div class="flex flex-col">
                <p>{{ title }}</p>
                <div class="flex justify-between">
                    <b>{{ price }}</b>
                    <img class="cursor-pointer" src="/close.svg" @click="emit('onClickRemove')"></img>
                </div>
            </div>
        </div>
</template>