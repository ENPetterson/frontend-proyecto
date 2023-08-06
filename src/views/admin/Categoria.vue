<template>
    <div class="card">
        <h5>Gestion Categorias</h5>

        
        <Button label="NUEVA CATEGORIA" icon="pi pi-external-link" @click="visible = true" />

        <Dialog v-model:visible="visible" modal header="Header" :style="{ width: '50vw' }" class="p-fluid">
            <label>Ingrese Nombre</label>      
            <InputText type="text" v-model="categoria.nombre" />
            <label>Ingrese Detalle</label>
            <Textarea v-model="categoria.detalle" rows="5" cols="30" />

            <button @click="guardarCategoria()">Guardar</button>
        </Dialog>
        <DataTable :value="categorias" tableStyle="min-width: 50rem">
            <Column field="id" header="ID"></Column>
            <Column field="nombre" header="NOMBRE"></Column>
            <Column field="detalle" header="DETALLE"></Column>
            <Column header="GESTION">
                <template #body="slotProps">
                    <Button icon="pi pi-pencil" class="p-button-rounded p-button-success mr-2" @click="editarCat(slotProps.data)" />
                    <Button icon="pi pi-trash" class="p-button-rounded p-button-warning mt-2" @click="eliminarCat(slotProps.data.id)" />
                </template>
            </Column>
            
        </DataTable>
        <!--pre>{{ categorias }}</pre-->
        <!--table border="1">
            <thead>
                <tr>
                    <td>ID</td>
                    <td>NOMBRE</td>
                    <td>DETALLE</td>
                    <td>GESTION</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="cat in categorias" :key="cat.id">
                    <td>{{ cat.id }}</td>
                    <td>{{ cat.nombre }}</td>
                    <td>{{ cat.detalle }}</td>
                    <td>
                        <button>Editar</button>
                        <button>Eliminar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    -->

        <Toast />

    </div>
</template>

<script setup>

//importaciones
import { ref, onMounted, guardReactiveProps } from "vue";
import categoriaService from "./../../services/categoria.service.js"
import { useToast } from 'primevue/usetoast';

const toast = useToast();

//variables o estados reactivos
    const categorias = ref([])
    const categoria = ref({nombre: '', detalle: ''})
    const visible = ref(false);

//metodos
onMounted(() => {
    getCategorias()
})
async function getCategorias() {
    const datos = await categoriaService.listar();

    categorias.value = datos.data
}

async function guardarCategoria() {
    try {
        if(categoria.value.id) {
            await categoriaService.modificar(categoria.value, categoria.value.id)

            getCategorias()
            visible.value = false
            categoria.value = {nombre: '', detalle: ''}
            toast.add({severity:'success', summary: 'Categoria Modificado', detail:'La categoria ha sido modificada ', life: 3000});

        } else {

            await categoriaService.guardar(categoria.value)

            getCategorias()
            visible.value = false
            categoria.value = {nombre: '', detalle: ''}
            toast.add({severity:'success', summary: 'Categoria Registrada', detail:'La categoria ha sido creada', life: 3000});
        }
    } catch (error) {
        alert(error)
    }
}

function editarCat(cat) {
    categoria.value = cat;
    visible.value = true
}

async function eliminarCat(id) {
    if(confirm("Esta seguro de eliminar la categoria?")) {
        await categoriaService.eliminar(id);
        getCategorias()
        toast.add({severity:'success', summary: 'Categoria Eliminada', detail:'La categoria ha sido eliminada', life: 3000});
    } else {

    }
}   

</script>