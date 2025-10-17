<script setup>
import { onMounted, ref } from 'vue'
import InventoryTable from '@/components/InventoryTable.vue'
import NewProductModal from '@/components/NewProductModal.vue'
import { inventoryApi } from './api/inventoryApi'

const products = ref([]) // initialize as an array (not null)
const showNewModal = ref(false)

const addProduct = async (newProduct) => {
  try {
    const data = {
      nombre: newProduct.nombre,
      descripcion: newProduct.descripcion
    }

    await inventoryApi.post('index.php', data)
  } catch (error) {
    console.error('Error al subir producto: ', error)
  }

  showNewModal.value = false
}

onMounted(async () => {
  try {
    const response = await inventoryApi.get('index.php')
    products.value = response.data.productos
    console.log('Productos cargados:', products.value)
  } catch (error) {
    console.error('Error al obtener productos: ', error)
  }
})
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
