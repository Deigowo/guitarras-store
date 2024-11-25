<script setup>
import {ref, reactive, computed, onMounted, watch} from 'vue'
import { db } from './data/guitarras'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitar from './components/Guitar.vue';

const guitarras = ref([])
const cart = ref([])
const guitar = ref({})

onMounted(() => {
    guitarras.value = db
    const cartStorage = localStorage.getItem('cart')
    if (cartStorage) 
        cart.value = JSON.parse(cartStorage)
    guitar.value = db[3]
})

function saveLocalStorage() {
    localStorage.setItem('cart', JSON.stringify(cart.value))
}

function addCart (guitar) {
    const idCart = cart.value.findIndex(g => g.id === guitar.id)
    if(idCart === -1) {
        guitar.cantidad = 1
        cart.value.push(guitar)
    } else {
        cart.value[idCart].cantidad++
    }
    saveLocalStorage()
}

function addOne(id) {
    const idCart = cart.value.findIndex(g => g.id === id)
    cart.value[idCart].cantidad++
    saveLocalStorage()
}

function deleteOne(id) {
    const idCart = cart.value.findIndex(g => g.id === id)
    cart.value[idCart].cantidad--
    if(cart.value[idCart].cantidad === 0) {
        cart.value = cart.value.filter(g => g.id !== id)
    }
    saveLocalStorage()
}

function deleteGuitar(id) {
    cart.value = cart.value.filter(g => g.id !== id)
    saveLocalStorage()
}

function emptyCart() {
    cart.value = []
    saveLocalStorage()
}

const total = computed(() => {
    return cart
        .value
        .reduce((total, g) => total + (g.precio * g.cantidad), 0)
})

watch(cart, saveLocalStorage, { deep: true })

</script>

<template>
    <Header 
    :cart="cart" 
    :total="total"
    :guitar="guitar"
    @add-Cart="addCart"
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