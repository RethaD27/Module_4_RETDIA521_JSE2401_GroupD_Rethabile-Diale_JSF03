<template>
  <nav class="fixed w-full bg-pink-600 p-4 text-pink-50 shadow-lg">
    <div class="container mx-auto flex justify-between items-center">
      <div class="text-lg font-bold">My FlexStore</div>
      <div class="hidden md:flex">
        <router-link to="/" class="mr-4 hover:underline">Home</router-link>
        <router-link to="/" class="mr-4 hover:underline">Wishlist</router-link>
        <router-link to="/" class="mr-4 hover:underline">
          <svg
            width="32px"
            height="32px"
            viewBox="0 0 32 32"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <!-- SVG content -->
          </svg>
        </router-link>
        <router-link to="/" class="hover:underline">Login</router-link>
      </div>
      <button @click="toggleMenu" class="md:hidden text-white">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16m-7 6h7"
          />
        </svg>
      </button>
    </div>
    <div v-if="open" class="md:hidden mt-4 bg-pink-500">
      <router-link to="/" class="block p-2 hover:bg-pink-400">Home</router-link>
      <router-link to="/" class="block p-2 hover:bg-pink-400">Wishlist</router-link>
      <router-link to="/" class="block p-2 hover:bg-pink-400">
        <svg
          width="32px"
          height="32px"
          viewBox="0 0 32 32"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <!-- SVG content -->
        </svg>
      </router-link>
      <router-link to="/" class="block p-2 hover:bg-pink-400">Login</router-link>
    </div>
  </nav>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'Navbar',
  setup() {
    const open = ref(false)

    const toggleMenu = () => {
      open.value = !open.value
    }

    return {
      open,
      toggleMenu
    }
  }
}
</script>