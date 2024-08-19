<script setup>

import { defineProps , defineEmits } from 'vue';
defineProps(['products'])

const emit = defineEmits(['add-to-cart']);
const addToCart =(item)=>{
    emit('add-to-cart',item)
};
</script>

<template>
    <div class="list-group">
        <div v-for="(item) in products" v-bind:key="item.id" @click="addToCart(item)">
        <a href="#" class="list-group-item list-group-item-action"
        ><div class="d-flex w-100 justify-content-between">
            <h5 class="mb-1">{{item.name}}</h5>
            <small>${{item.price}}</small>
        </div>
        <p class="mb-1">{{item.description}}</p>
        </a>
        </div>
    </div>
</template>