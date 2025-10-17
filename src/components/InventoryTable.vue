<script setup>
import { defineProps, defineEmits } from 'vue'
import { DataTable, Column, Button } from 'primevue'

const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['edit'])

const onEdit = (item) => {
  emit('edit', item)
}
</script>

<template>
  <DataTable
    :value="items"
    stripedRows
    showGridlines
    responsiveLayout="scroll"
    class="shadow-md rounded-2xl"
  >
    <Column field="id" header="ID" />
    <Column field="nombre" header="Nombre" />
    <Column field="descripcion" header="Descripción" />

    <!-- Action column using template slot -->
    <Column header="Acciones">
      <template #body="{ data }">
        <Button
          label="Editar"
          icon="pi pi-pencil"
          class="p-button-sm p-button-rounded p-button-info"
          @click="onEdit(data)"
        />
      </template>
    </Column>
  </DataTable>
</template>
