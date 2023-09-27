<script setup>
import { onMounted, ref } from 'vue'
import ProductForm from '../components/ProductForm.vue'
import router from '../router'
import axios from 'axios'
import { useRoute } from 'vue-router'
const route = useRoute()

const product = ref({
  name: '',
  price: 0,
  cost: 0,
  quantity: 0
})

function OnSubmitProduct(product) {
  axios.put(`https://api.budu.triple-i.in/products/${route.params.id}`, {
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

function loadProduct() {
  axios.get(`https://api.budu.triple-i.in/products/${route.params.id}`).then(function (response) {
    // handle success
    product.value = response.data
  })
}

onMounted(() => {
  loadProduct()
});
</script>

<template>
  <main class="w-full">
    <h1 class="text-3xl">Edit Product Form</h1>
    <ProductForm :product="product" @submit="OnSubmitProduct" @reset="OnResetProduct" />
  </main>
</template>
