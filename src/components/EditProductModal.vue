<script setup>
import { Button, Dialog, InputText } from 'primevue'
import { ref, watch } from 'vue'
import { inventoryApi } from '@/api/inventoryApi'

const props = defineProps({
  product: { type: Object, required: true }
})
const emit = defineEmits(['save', 'delete', 'close'])

const editableProduct = ref({ ...props.product })

// Sync if another product is selected
watch(() => props.product, (newVal) => {
  editableProduct.value = { ...newVal }
})

const updateProduct = async () => {
  try {
    await inventoryApi.patch(`productos/${editableProduct.value.id}`, {
      nombre: editableProduct.value.nombre,
      descripcion: editableProduct.value.descripcion
    })

    console.log({editableProduct});
    
    emit('save') // tell parent to refresh
  } catch (error) {
    console.error('Error updating product:', error)
  }
}

const deleteProduct = async () => {
  try {
    await inventoryApi.delete(`productos/${editableProduct.value.id}`)
    emit('delete') // tell parent to refresh
  } catch (error) {
    console.error('Error deleting product:', error)
  }
}
</script>

<template>
  <Dialog
    header="Editar Producto"
    modal
    :visible="true"
    :style="{ width: '25rem' }"
    @hide="emit('close')"
  >
    <div class="flex flex-col gap-4 mt-4">
      <div>
        <label class="font-semibold block mb-1">ID</label>
        <InputText v-model="editableProduct.id" disabled class="w-full" />
      </div>

      <div>
        <label class="font-semibold block mb-1">Nombre</label>
        <InputText v-model="editableProduct.nombre" class="w-full" />
      </div>

      <div>
        <label class="font-semibold block mb-1">Descripción</label>
        <InputText v-model="editableProduct.descripcion" class="w-full" />
      </div>
    </div>

    <template #footer>
      <div class="flex justify-between w-full mt-4">
        <Button
          label="Eliminar"
          icon="pi pi-trash"
          class="p-button-danger"
          @click="deleteProduct"
        />
        <div class="flex gap-2">
          <Button
            label="Cancelar"
            icon="pi pi-times"
            class="p-button-text"
            @click="emit('close')"
          />
          <Button
            label="Aceptar"
            icon="pi pi-check"
            class="p-button-success"
            @click="updateProduct"
          />
        </div>
      </div>
    </template>
  </Dialog>
</template>
