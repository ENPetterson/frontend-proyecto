<script setup>
import { ref } from 'vue';

import AppMenuItem from './AppMenuItem.vue';

const model = ref([
    {
        label: 'Administración',
        items: [{ label: 'admin', icon: 'pi pi-fw pi-home', to: '/' }, 
        { label: 'perfil', icon: 'pi pi-fw pi-user', to: '/about' }],
        
    },
    {
        label: 'Gestión Productos',
        items: [
            { label: 'Categorias', icon: 'pi pi-fw pi-id-card', to: '/categoria' },
            { label: 'Productos', icon: 'pi pi-fw pi-check-square', to: '/producto' }
        ]
    },
    {
        label: 'Gestion Pedidos',
        items: [
            { label: 'Cliente', icon: 'pi pi-fw pi-eye', to: '/cliente', badge: 'NEW' },
            { label: 'Pedidos', icon: 'pi pi-fw pi-globe', to: '/pedido' },
            { label: 'Nuevo Pedido', icon: 'pi pi-fw pi-globe', to: '/pedido/nuevo' }
        ]
    },
    {
        label: 'Gestion Usuarios',
        items: [
            { label: 'Usuarios', icon: 'pi pi-fw pi-prime', to: '/utilities/icons' },
        ]
    },
]);
</script>

<template>
    <ul class="layout-menu">
        <template v-for="(item, i) in model" :key="item">
            <app-menu-item v-if="!item.separator" :item="item" :index="i"></app-menu-item>
            <li v-if="item.separator" class="menu-separator"></li>
        </template>
        <!--li>
            <a href="https://www.primefaces.org/primeblocks-vue/#/" target="_blank">
                <img src="/layout/images/banner-primeblocks.png" alt="Prime Blocks" class="w-full mt-3" />
            </a>
        </li-->
    </ul>
</template>

<style lang="scss" scoped></style>
