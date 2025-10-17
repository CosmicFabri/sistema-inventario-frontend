<script setup>
import { onMounted, ref } from 'vue'
import InventoryTable from '@/components/InventoryTable.vue'
import NewProductModal from '@/components/NewProductModal.vue'
import inventoryData from '@/data/inventoryData.js'

const products = ref([...inventoryData])
const showNewModal = ref(false)

function addProduct(newProduct) {
  // Generate next ID automatically
  const nextId =
    products.value.length > 0
      ? Math.max(...products.value.map(p => p.id)) + 1
      : 1

  products.value.push({
    id: nextId,
    nombre: newProduct.nombre,
    descripcion: newProduct.descripcion
  })
  showNewModal.value = false
}
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gray-50 p-6">
    <h1 class="text-4xl font-semibold mb-8 text-gray-800">Sistema de Inventario</h1>

    <div class="w-full max-w-3xl flex justify-end mb-4">
      <button
        @click="showNewModal = true"
        class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition"
      >
        Nuevo Producto
      </button>
    </div>

    <div class="w-full max-w-3xl">
      <InventoryTable :items="products" />
    </div>

    <NewProductModal
      v-if="showNewModal"
      @save="addProduct"
      @close="showNewModal = false"
    />
  </div>
</template>

<style scoped></style>
