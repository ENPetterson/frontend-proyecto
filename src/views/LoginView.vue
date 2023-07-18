<template>

    <div>
        {{ usuario }}
        <h1>Login</h1>
        <label for="e">Ingresar Correo:</label>
        <input type="email" v-model="usuario.email">
        <br>
        <label>Ingresar Contraseña:</label>
        <input type="password" v-model="usuario.password">
        <br>
        <input type="button" value="Ingresar" v-on:click="funIngresar()">
    </div>

</template>

<script setup>
    import { ref } from "vue";
    import axios from "axios";

    /*
    const correo = ref("aa@gmail.com");
    const clave = ref("aa");
    */
    const usuario = ref({email: "", password: ""});

    function funIngresar() {
        axios.post("http://127.0.0.1:8000/api/v1/auth/login", usuario.value).then(
            (res) => {
                console.log(res)
            }
        )
    }

</script>


<style>

</style>