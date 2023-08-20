<template>
  <h1>Tabla componente de producto</h1>

  <DataTable :value="props.data">
    <Column field="id" header="ID"></Column>
    <Column field="nombre" header="Nombre"></Column>
    <Column field="precio" header="Precio"></Column>
    <Column field="cantidad" header="Cantidad"></Column>
    <Column headerStyle="min-width:3rem;" field="cantidad" header="Gestion">
      <template #body="slotProps">
        <Button
          icon="pi pi-plus"
          class="p-button-rounded p-button-warning mr-2"
          @click="agregar(slotProps.data)"
        />
      </template>
    </Column>
  </DataTable>
</template>

<script setup>
const props = defineProps(["nombre", "data"]);
const emit = defineEmits(["funCarrito"]);

function agregar(prod) {
  emit("funCarrito", prod);
}
</script>
