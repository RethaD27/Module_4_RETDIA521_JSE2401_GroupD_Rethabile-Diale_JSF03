<template>
  <div class="container mx-auto p-6 relative"> <!-- Added relative positioning -->
    <button @click="goBack" class="absolute top-4 left-4 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-blue-300 focus:ring-opacity-75 transition duration-200">
      Go Back
    </button>
    
    <div v-if="loading" class="flex items-center justify-center h-screen">
      <div class="text-center py-8 bg-white p-4 rounded shadow-md">
        <svg class="w-16 h-16 mx-auto animate-spin" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12c0-4.418 3.582-8 8-8s8 3.582 8 8H4z"></path>
        </svg>
        <p class="mt-4">Loading...</p>
      </div>
    </div>
    
    <div v-else-if="product" class="bg-white p-6 rounded shadow-lg mt-16"> <!-- Added top margin to accommodate the button -->
      <div class="flex justify-center items-center">
        <img :src="product.image" :alt="product.title" class="w-64 h-64 object-contain mb-4 rounded" />
      </div>
      <h3 class="text-2xl font-bold mb-2">{{ product.title }}</h3>
      <p class="text-gray-700 mb-2">${{ product.price.toFixed(2) }}</p>
      <p class="text-gray-500 mb-2">Category: {{ product.category }}</p>
      <p class="text-gray-700 mb-2">
        Rating: {{ product.rating.rate }} ({{ product.rating.count }} reviews)
      </p>
      <p class="text-gray-700 mb-4">{{ product.description }}</p>
      <div class="flex justify-evenly items-center mt-5">
        <button @click="toggleFavorite(product.id)" class="">
          <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="w-6 h-6" :class="{ 'text-red-500': isFavorite(product.id), 'text-gray-300': !isFavorite(product.id) }" viewBox="0 0 24 24">
            <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" />
          </svg>
        </button>
        <button class="bg-pink-500 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-pink-400 focus:ring-opacity-75 transition duration-200">
          Add To Cart +
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default {
  name: 'ProductDetail',
  setup() {
    const route = useRoute()
    const router = useRouter()
    const product = ref(null)
    const loading = ref(true)
    const favorites = ref([])

    onMounted(async () => {
      loading.value = true
      try {
        const response = await fetch(`https://fakestoreapi.com/products/${route.params.id}`)
        const data = await response.json()
        product.value = data
      } catch (error) {
        console.error('Error fetching product details:', error)
      } finally {
        loading.value = false
      }

      const storedFavorites = localStorage.getItem('favorites')
      if (storedFavorites) {
        favorites.value = JSON.parse(storedFavorites)
      }
    })

    function goBack() {
      router.go(-1)
    }

    function toggleFavorite(productId) {
      const index = favorites.value.indexOf(productId)
      if (index > -1) {
        favorites.value.splice(index, 1)
      } else {
        favorites.value.push(productId)
      }
      localStorage.setItem('favorites', JSON.stringify(favorites.value))
    }

    function isFavorite(productId) {
      return favorites.value.includes(productId)
    }

    return {
      product,
      loading,
      goBack,
      toggleFavorite,
      isFavorite
    }
  }
}
</script>