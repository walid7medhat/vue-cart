<script setup lang="ts">
import { ref } from 'vue'
import { useCartStore } from '@/store/cart'
import type { Product } from '@/store/products'
import { toCurrency } from '@/shared/utils'

defineProps<{
  product: Product
}>()

const cartStore = useCartStore()
const notificationMessage = ref('')

function addToCart(product: Product) {
  cartStore.add(product.id)
  notificationMessage.value = `${product.title} added to cart successfully!`

  setTimeout(() => {
    notificationMessage.value = ''
  }, 3000) // Display for 3 seconds
}
</script>

<template>
  <div class="card bordered">
    <figure class="px-8 pt-10">
      <img
        :src="product.image"
        alt="Card Image"
        class="object-contain w-full h-64"
      >
    </figure>
    <div class="card-body">
      <h2 class="card-title">
        <router-link class="link link-hover" :to="`/product/${product.id}`">
          {{ product.title }}
        </router-link>
      </h2>
      <p>{{ toCurrency(product.price) }}</p>
      <div class="justify-end card-actions">
        <button class="btn btn-primary" @click="addToCart(product)">
          Add to Cart
        </button>
      </div>
    </div>
    <transition name="slide-fade">
      <div v-if="notificationMessage" class="fixed top-0 left-0 right-0 py-2 text-center text-white bg-green-500">
        {{ notificationMessage }}
      </div>
    </transition>
  </div>
</template>

<style scoped>
.slide-fade-enter-active, .slide-fade-leave-active {
  transition: all 0.5s ease;
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
