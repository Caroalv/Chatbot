<template>
  <div class="p-6 max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
    <h1 class="text-2xl font-bold text-center text-gray-800">My App 1</h1>

    <section class="text-center">
      <h3 class="text-lg font-medium text-gray-700">Número = {{ numero }}</h3>
    </section>

    <div class="flex justify-center space-x-4">
      <button
        @click="inc"
        class="btn"
      >
        +1
      </button>
      <button
        @click="dec"
        class="btn"      >
        -1
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const numero = ref(0)

const inc = () => {
  numero.value++
}
const dec = () => {
  numero.value--
}
</script>
