<template>
  <div class="grid">
    <div class="col-7">
      <div class="card">
        <tabla-producto-component
          :nombre="nombre"
          :data="productos"
          @funCarrito="addCarrito"
        />
      </div>
    </div>

    <div class="col-5">
      <div class="grid">
        <div class="col-12">
          <div class="card">
            <DataTable :value="carrito">
              <Column field="nombre" header="Nombre"></Column>
              <Column field="precio" header="Precio"></Column>
              <Column field="cantidad" header="Cantidad">
                <template #body="slotProps">
                  {{ slotProps.data.cantidad }}
                  <Button
                    icon="pi pi-minus"
                    class="p-button-rounded p-button-danger mr-2"
                    @click="reducirCant(slotProps.data)"
                  />
                  <Button
                    icon="pi pi-plus"
                    class="p-button-rounded p-button-success mr-2"
                    @click="incrementarCant(slotProps.data)"
                  />
                </template>
              </Column>
              <Column headerStyle="min-width:3rem;">
                <template #body="slotProps">
                  <Button
                    icon="pi pi-trash"
                    class="p-button-rounded p-button-danger mr-2"
                    @click="quitarCarrito(slotProps.data)"
                  />
                </template>
              </Column>
            </DataTable>
          </div>
        </div>
        <div class="col-12">
          <div class="card">

            <InputText
                  v-model.trim="buscarClie"
                  placeholder="Buscar.."
                  @keypress.enter="buscarCliente"
                />

            <Button
              label="Nuevo Cliente"
              icon="pi pi-user"
              @click="visibleCliente = true"
            />
            <div class="card" v-if="cliente_seleccionado.id">
                <h4>Cliente: {{ cliente_seleccionado.nombre_completo }}</h4>
                <h4>CI/NIT: {{ cliente_seleccionado.ci_nit }}</h4>
                <h4>Telefono: {{ cliente_seleccionado.telefono }}</h4>
            </div>
            <Dialog
              v-model:visible="visibleCliente"
              modal
              header="Nuevo Cliente"
              :style="{ width: '50vw' }"
              class="p-fluid"
            >
              <div class="field">
                <label for="name">Nombre Completo</label>
                <InputText
                  id="name"
                  v-model.trim="cliente.nombre_completo"
                  required="true"
                />
              </div>
              <div class="field">
                <label for="ci_nit">CI/NIT</label>
                <InputText
                  id="ci_nit"
                  v-model.trim="cliente.ci_nit"
                />
              </div>
              <div class="field">
                <label for="telefono">Telefono</label>
                <InputText
                  id="telefono"
                  v-model.trim="cliente.telefono"
                />
              </div>

              <template #footer>
                <Button
                  label="Cancelar"
                  icon="pi pi-times"
                  class="p-button-text"
                  @click="visibleCliente=false"
                />
                <Button
                  label="Guardar Cliente"
                  icon="pi pi-check"
                  class="p-button-text"
                  @click="guardarCliente"
                />
              </template>
            </Dialog>
          </div>
        </div>
        <div class="col-12">
          <div class="card">
            <Button
              label="Registrar Pedido"
              icon="pi pi-save"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import TablaProductoComponent from "@/components/admin/TablaProductoComponent.vue";
import { onMounted, ref } from "vue";
import productoService from "../../../services/producto.service";
import clienteService from "../../../services/cliente.service";

const nombre = ref("Lista Productos");
const productos = ref([]);
const carrito = ref([]);
const visibleCliente = ref(false);
const cliente = ref({});
const cliente_seleccionado = ref({id: 1, nombre_completo: "Juan Carlos", telefono: 1234})
const buscarClie = ref("")

onMounted(() => {
  listarProducto();
});

const listarProducto = async () => {
  const { data } = await productoService.listar();
  productos.value = data.data;
};

const addCarrito = (prod) => {
  const p = {
    producto_id: prod.id,
    nombre: prod.nombre,
    precio: prod.precio,
    cantidad: 1,
    stock: prod.cantidad
  };
  carrito.value.push(p);
};

const quitarCarrito = (prod) => {
  console.log(prod.producto_id);
  carrito.value.splice(carrito.value.indexOf(prod), 1);
};

const incrementarCant = (prod) => {
    if(prod.cantidad < prod.stock)
     prod.cantidad++;
};

const reducirCant = (prod) => {
    if(prod.cantidad >= 2)
     prod.cantidad--;
};

const guardarCliente = async () => {
    const {data} = await clienteService.guardar(cliente.value)
    cliente_seleccionado.value = data.cliente;

    visibleCliente.value = false
    cliente.value = {}
};

const buscarCliente = async () => {
    const {data} = await clienteService.buscar(buscarClie.value)
    cliente_seleccionado.value = data;
}

</script>