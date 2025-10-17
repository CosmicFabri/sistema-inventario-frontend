<script setup>
import { defineProps } from 'vue'
import { DataTable, Column } from 'primevue'

const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})
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
  </DataTable>
</template>
