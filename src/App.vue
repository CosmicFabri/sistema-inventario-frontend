<script setup>
import { ref, onMounted } from 'vue'
import InventoryTable from '@/components/InventoryTable.vue'
import NewProductModal from '@/components/NewProductModal.vue'
import EditProductModal from '@/components/EditProductModal.vue'
import { inventoryApi } from './api/inventoryApi'

const products = ref([])
const showNewModal = ref(false)
const showEditModal = ref(false)
const selectedProduct = ref(null)

const fetchProducts = async () => {
  try {
    const response = await inventoryApi.get('index.php')
    products.value = response.data.productos
  } catch (error) {
    console.error('Error al obtener productos:', error)
  }
}

const addProduct = async (newProduct) => {
  try {
    await inventoryApi.post('index.php', newProduct)
    await fetchProducts()
  } catch (error) {
    console.error('Error al subir producto:', error)
  } finally {
    showNewModal.value = false
  }
}

const openEditModal = (product) => {
  selectedProduct.value = product
  showEditModal.value = true
}

const handleSaveEdit = async () => {
  await fetchProducts()
  showEditModal.value = false
}

const handleDelete = async () => {
  await fetchProducts()
  showEditModal.value = false
}

onMounted(fetchProducts)
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
      <InventoryTable :items="products" @edit="openEditModal" />
    </div>

    <NewProductModal
      v-if="showNewModal"
      @save="addProduct"
      @close="showNewModal = false"
    />

    <EditProductModal
      v-if="showEditModal"
      :product="selectedProduct"
      @save="handleSaveEdit"
      @delete="handleDelete"
      @close="showEditModal = false"
    />
  </div>
</template>
