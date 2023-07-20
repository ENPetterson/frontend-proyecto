<template>

    <div>
        {{ usuario }}
        <h1>Login</h1>
        <label for="e">Ingresar Correo:</label>
        <input type="email" v-model="usuario.email">
        <span>{{ errors.email }}</span>
        <br>
        <label>Ingresar Contraseña:</label>
        <input type="password" v-model="usuario.password">
        <span>{{ errors.password }}</span>
        <br>
        <input type="button" value="Ingresar" v-on:click="funIngresar()">


    </div>

</template>

<script setup>
    import { ref } from "vue";
    import axios from "axios";
    import { useRouter } from 'vue-router';
    import authService from "./../services/auth.service";

    /*
    const correo = ref("aa@gmail.com");
    const clave = ref("aa");
    */
    const usuario = ref({email: "", password: ""});
    const errors = ref({})

    const router = useRouter()

    async function funIngresar() {
        try {
            const { data } = await authService.loginConLaravel(usuario.value)
            console.log("CON INTERCEPTOR: ", data)
            errors.value = {}
                
            localStorage.setItem("access_token", data.access_token);
            router.push("/about")
            /*
            if(data.access_token) {
            } else {
                alert("Error al autenticar")
            }
            */
        } catch (error) {
            console.log(error.response.data)
            errors.value = error.response.data.errors
        }

        /*
        axios.post("http://127.0.0.1:8000/api/v1/auth/login", usuario.value).then(
            (res) => {
                console.log("CON AXIOS: ", res.data)
            }
        )
        */

        
    }

</script>


<style>

</style>