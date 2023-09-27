<script setup>
import { ref } from 'vue'
import ProductForm from '../components/ProductForm.vue'
import router from '../router'
import axios from 'axios'

const product = ref({
  name: '',
  price: 0,
  cost: 0,
  quantity: 0
})

function OnSubmitProduct(product) {
  axios.post(`https://api.budu.triple-i.in/products`, {
    product
  }).then(function (response) {
    // handle success
    product.value = response.data
    router.push('/')
  })
}

function OnResetProduct() {
  product.value = {
    name: '',
    price: 0,
    cost: 0,
    quantity: 0
  }
}
</script>

<template>
  <main class="w-full">
    <h1 class="text-3xl">New Product Form</h1>
    <ProductForm :product="product" @submit="OnSubmitProduct" @reset="OnResetProduct" />
  </main>
</template>
