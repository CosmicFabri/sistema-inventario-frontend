<script setup>
import { onMounted, ref, watch } from 'vue'
import { DataTable } from 'primevue'
import { Column } from 'primevue'
import { Button } from 'primevue'
import EditProductModal from '@/components/EditProductModal.vue'

const props = defineProps({
  items: { type: Array, required: true }
})

const itemsRef = ref([...props.items])
const selectedProduct = ref(null)
const isModalVisible = ref(false)

watch(
  () => props.items,
  (newItems) => {
    itemsRef.value = newItems
  },
  { deep: true }
)

function openEditModal(product) {
  selectedProduct.value = { ...product }
  isModalVisible.value = true
}

function handleSave(updatedProduct) {
  const index = itemsRef.value.findIndex(p => p.id === updatedProduct.id)
  if (index !== -1) itemsRef.value[index] = { ...updatedProduct }
  isModalVisible.value = false
}

function handleDelete(productId) {
  const index = itemsRef.value.findIndex(p => p.id === productId)
  if (index !== -1) itemsRef.value.splice(index, 1)
  isModalVisible.value = false
}

onMounted(async () => {
    
})
</script>

<template>
  <div class="w-full">
    <DataTable
      :value="itemsRef"
      size="small"
      class="shadow-md rounded-2xl"
      stripedRows
      showGridlines
      responsiveLayout="scroll"
    >
      <Column field="id" header="ID" style="width: 8rem"></Column>
      <Column field="nombre" header="Nombre"></Column>
      <Column field="descripcion" header="Descripción"></Column>

      <Column header="Acciones" style="width: 8rem; text-align:center;">
        <template #body="slotProps">
          <Button
            label="Editar"
            icon="pi pi-pencil"
            size="small"
            class="p-button-rounded p-button-info"
            @click="openEditModal(slotProps.data)"
          />
        </template>
      </Column>
    </DataTable>

    <EditProductModal
      v-if="isModalVisible"
      :product="selectedProduct"
      @save="handleSave"
      @delete="handleDelete"
      @close="isModalVisible = false"
    />
  </div>
</template>

<style scoped></style>
