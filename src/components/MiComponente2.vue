<template>
  <div class="p-6 max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
    <h1 class="text-2xl font-bold text-center text-gray-800">Componente 2</h1>

    <div class="space-y-2 text-center">
      <h3 class="text-lg font-medium text-gray-700">Número: {{ numero }}</h3>
      <h3 class="text-lg font-medium text-gray-700">Multiplicar: {{ contador }}</h3>
    </div>

    <div class="flex justify-center space-x-4">
      <button
        @click="numero++"
        class="btn"      >
        +1
      </button>
      <button
        @click="numero--"
        class="btn"      >
        -1
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'

interface props {
  value: number
}

const Props = defineProps<props>()

const numero = ref(Props.value)

const contador = computed(() => numero.value * numero.value)
</script>
