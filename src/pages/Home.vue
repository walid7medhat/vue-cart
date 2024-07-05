<script setup lang="ts">
import { computed } from 'vue'
import ProductCard from '@/components/ProductCard.vue'
import ProductCardSkeleton from '@/components/ProductCardSkeleton.vue'
import { useProductStore } from '@/store/products'

const productStore = useProductStore()

const products = computed(() => productStore.list)
</script>

<template>
  <div class="p-6 mx-auto max-w-7xl bg-gray-50">
    <div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
      <ProductCardSkeleton
        v-for="n in 15"
        v-show="!productStore.loaded"
        :key="n"
        class="rounded-lg shadow-md"
      />
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        class="transition-transform duration-300 transform rounded-lg shadow-lg hover:scale-105 hover:shadow-2xl"
      />
    </div>
  </div>
</template>
