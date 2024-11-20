<script setup>
import {ref, reactive, computed, watch} from 'vue'
import { db } from './data/guitarras'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitar from './components/Guitar.vue';

const guitarras = ref(db)
const cart = ref([])

function addCart (guitar) {
    const idCart = cart.value.findIndex(g => g.id === guitar.id)
    if(idCart === -1) {
        guitar.cantidad = 1
        cart.value.push(guitar)
    } else {
        cart.value[idCart].cantidad++
    }
}

function addOne(id) {
    const idCart = cart.value.findIndex(g => g.id === id)
    cart.value[idCart].cantidad++
}

function deleteOne(id) {
    const idCart = cart.value.findIndex(g => g.id === id)
    cart.value[idCart].cantidad--
    if(cart.value[idCart].cantidad === 0) {
        cart.value = cart.value.filter(g => g.id !== id)
    }
}

function deleteGuitar(id) {
    cart.value = cart.value.filter(g => g.id !== id)
}

function emptyCart() {
    cart.value = []
}

const total = computed(() => {
    let total = 0
    cart.value.forEach(g => {
        total += g.cantidad * g.precio
    })
    return total
})

watch(cart, total)
</script>

<template>
    <Header 
    :cart="cart" 
    :total="total"
    @add-One="addOne"
    @delete-One="deleteOne"
    @delete-Guitar="deleteGuitar"
    @empty-Cart="emptyCart"
    />

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