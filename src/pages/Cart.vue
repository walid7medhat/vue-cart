<script setup lang="ts">
import { computed } from 'vue'
import CartCard from '../components/CartCard.vue'
import CartCardSkeleton from '../components/CartCardSkeleton.vue'
import { toCurrency } from '../shared/utils'
import { useCartStore } from '../store/cart'
import { useProductStore } from '../store/products'

const cartStore = useCartStore()
const productStore = useProductStore()

const formattedCart = computed(() => cartStore.formattedCart)
 
function clearCart() {
  cartStore.clearCart() // Now the clearCart method exists on the cartStore object
}

</script>

<template>
  <div class="max-w-4xl p-4 mx-auto">
    <div v-if="!productStore.loaded" class="space-y-4">
      <CartCardSkeleton v-for="n in 15" :key="n" />
    </div>
    <div v-else-if="!formattedCart.length">
      <h1 class="text-xl">
        Cart is empty.
      </h1>
    </div>
    <div v-else class="space-y-4">
      <div class="flex justify-end">
        <button class="btn btn-danger" @click="clearCart">
          Clear Cart
        </button>
      </div>
      <CartCard
        v-for="(cartProduct, index) in formattedCart"
        :key="index"
        :cart-product="cartProduct"
      />
      <div class="text-2xl text-right md:text-4xl">
        Total: {{ toCurrency(cartStore.total) }}
      </div>
    </div>
  </div>
</template>
