<script setup>
import { ref, reactive, onMounted, watch } from 'vue'
import {db} from './data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

// este es el metodo reactivo para el estado
// const state = reactive({
//     guitarras:db
// })
// para llamar la informacion se usa state.guitarras
// console.log(state.guitarras);

// este es el metodo ref para el estado
const guitarras = ref([])
// para llamar la informacion se usa guitarras.value
// console.log(guitarras.value)

const carrito = ref([])
const guitarra = ref({})

watch(carrito, ()=>{
    guardarLocalStorage()
}, {deep:true})

// deep:true es para que el watch este pendiente de los cambios internos en el carrito, de cada propiedad del objeto
// deep tiene complicaciones con archivos grandes, por lo que si sientes lento el sitio puede ser por eso.

onMounted(() => {
    guitarras.value = db
    guitarra.value = db[3]

    const carritoLocal = localStorage.getItem('carrito')
    if(carritoLocal){
        carrito.value = JSON.parse(carritoLocal)
    }
})

const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

// recibe la guitarra que se agrega al carrito desde el hijo
const incrementar = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);

    if(existeCarrito >= 0){
        carrito.value[existeCarrito].cantidad++;
        return;
    }else{
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
}

const decrementarCantidad = (id)=>{
    const index = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[index].cantidad <= 1) return
    carrito.value[index].cantidad--;
}
const incrementarCantidad = (id)=>{
    const index = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[index].cantidad >= 5) return
    carrito.value[index].cantidad++
}

const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
    carrito.value = []
}

</script>

<template>
    <Header 
        :carrito="carrito"
        :guitarra="guitarra"
        @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad"
        @agregar-carrito="incrementar"
        @eliminar-producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <!-- aca -->
            <Guitarra 
                v-for="guitarra in guitarras"
                :guitarra="guitarra"
                @agregar-carrito="incrementar"
            />
            <!-- para pasar una funcion en el componente se hace mediante emit -->
            <!-- para esto en el componente se debe pasar la funcion con @ se declara la funcion -->
            <!-- se pasa el nombre de la funcion despues del igual entre las comillas -->
            <!-- y despues en el componente se llama con $emit('nombre de la funcion') -->
            <!-- si el emit tiene doble nombre este se separa por un "-", la funcion no puede usar esta nomenclatura  -->
        </div>
    </main>
    <Footer />
</template>

