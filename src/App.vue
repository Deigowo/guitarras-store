<script setup>
import {ref, reactive} from 'vue'
import { db } from './data/guitarras'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitar from './components/Guitar.vue';

const guitarras = ref(db)
const cart = ref([])

function addCart () {
    console.log('Producto agregado al carrito')
    //Insertar objetos al carrito
    cart.value.push(guitarras)
}

</script>

<template>
    <Header :cart="cart" />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5"> 
            <Guitar 
                v-for="guitar in guitarras"
                :guitar="guitar"
                :key="guitar.id"
                @addCart="addCart"
            />
        </div>
    </main>

    <Footer />
</template>

<style scoped>

</style>