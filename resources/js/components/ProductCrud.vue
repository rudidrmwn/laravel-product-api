<template>
  <div class="p-4">
    <h1 class="text-xl font-bold mb-4">Product List</h1>

    <form @submit.prevent="saveProduct" class="mb-4">
      <input v-model="form.name" placeholder="Product Name" class="border p-2 mr-2" />
      <input v-model="form.price" placeholder="Price" type="number" class="border p-2 mr-2" />
      <button type="submit" class="bg-blue-500 text-white p-2">Save</button>
    </form>

    <ul>
      <li v-for="product in products" :key="product.id" class="mb-2">
        {{ product.name }} - Rp{{ product.price }}
        <button @click="editProduct(product)" class="ml-2 text-yellow-500">Edit</button>
        <button @click="deleteProduct(product.id)" class="ml-2 text-red-500">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const products = ref([])
const form = ref({ id: null, name: '', price: '' })

const getProducts = async () => {
  const res = await axios.get('/api/products')
  products.value = res.data
}

const saveProduct = async () => {
  if (form.value.id) {
    await axios.put(`/api/products/${form.value.id}`, form.value)
  } else {
    await axios.post('/api/products', form.value)
  }
  form.value = { id: null, name: '', price: '' }
  getProducts()
}

const editProduct = (product) => {
  form.value = { ...product }
}

const deleteProduct = async (id) => {
  await axios.delete(`/api/products/${id}`)
  getProducts()
}

onMounted(getProducts)
</script>
