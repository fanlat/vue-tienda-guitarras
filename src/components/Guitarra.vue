<script setup>

// para definir los props se usa el defineProps
 const props = defineProps({
     guitarra: {
        type:Object,
        required:true
     }
 })

//  para definir una funcion que se pasa se usa el defineEmits
defineEmits(['agregar-carrito'])

</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img class="img-fluid" :src="'/img/'+guitarra.imagen+'.jpg'" :alt="guitarra.nombre">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{guitarra.nombre}}</h3>
            <p>{{guitarra.descripcion}}</p>
            <p class="fw-black text-primary fs-3">${{guitarra.precio}}</p>
            <button 
                type="button"
                class="btn btn-dark w-100 "
                @click="$emit('agregar-carrito', guitarra)"
            >Agregar al Carrito</button>
            <!-- para llamar la funcion con el emit se usa el $emit('nombre de la funcion') -->
            
            <!-- para pasar informacion del hijo al padre lo podemos hacer  por el emit -->
            <!-- solo debemos agregar despues del nombre de la función los datos que deseamos enviar -->
            <!-- y en la funcion del padre lo reciviomos como parametro -->

        </div>
    </div><!-- FIN GUITARRA -->
</template>


<style lang="scss" scoped>

</style>