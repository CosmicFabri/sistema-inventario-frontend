<script setup>
import { ref } from 'vue'
import { Dialog } from 'primevue'
import { InputText } from 'primevue'
import { Button } from 'primevue'

const emit = defineEmits(['save', 'close'])

const nombre = ref('')
const descripcion = ref('')

const submit = () => {
  const product = {
    nombre: nombre.value,
    descripcion: descripcion.value
  }

  emit('save', product)  // send product to parent
  nombre.value = ''
  descripcion.value = ''
}
</script>

<template>
  <Dialog
    header="Nuevo Producto"
    modal
    :visible="true"
    :style="{ width: '25rem' }"
    @hide="emit('close')"
  >
    <div class="flex flex-col gap-4 mt-4">
      <div>
        <label class="font-semibold block mb-1">Nombre</label>
        <InputText v-model="nombre" class="w-full" placeholder="Nombre del producto" />
      </div>

      <div>
        <label class="font-semibold block mb-1">Descripción</label>
        <InputText v-model="descripcion" class="w-full" placeholder="Descripción del producto" />
      </div>
    </div>

    <template #footer>
      <div class="flex justify-end gap-2 w-full mt-4">
        <Button
          label="Cancelar"
          icon="pi pi-times"
          class="p-button-text"
          @click="emit('close')"
        />
        <Button
          label="Guardar"
          icon="pi pi-check"
          class="p-button-success"
          @click="submit"
        />
      </div>
    </template>
  </Dialog>
</template>
