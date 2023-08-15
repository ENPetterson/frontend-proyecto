<template>
    <h1>Tabla componente de producto</h1>

    <DataTable :value="props.data">
        <Column field="id" header="ID"></Column>
        <Column field="nombre" header="Nombre"></Column>
        <Column field="precio" header="Precio"></Column>
        <Column field="cantidad" header="Cantidad"></Column>
    </DataTable>

</template>

<script setup>
   const props = defineProps(['nombre', 'data'])
</script>
