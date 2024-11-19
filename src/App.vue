<script setup>
import {ref, reactive} from 'vue'
import { db } from './data/guitarras'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitar from './components/Guitar.vue';

const guitarras = ref(db)


</script>

<template>
    <Header />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5"> 
            <Guitar 
                v-for="guitar in guitarras"
                :guitar="guitar"
                :key="guitar.id"
            />
        </div>
    </main>

    <Footer />
</template>

<style scoped>

</style>