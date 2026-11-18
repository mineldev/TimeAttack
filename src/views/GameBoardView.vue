<script setup>
import { ref } from 'vue'
import KeyPad from '@/components/KeyPad.vue'
const tecles = ['A', 'S', 'D', 'F']

let tempsInici = 0
const tempsObjectiu = {
  A: 2,
  S: 4,
  D: 5,
  F: 7
}

const jugant = ref(false)
const resultats = ref([])

function iniciarPartida(){
    resultats.value = [];
    jugant.value = true;
    tempsInici = performance.now(); 
    console.log(tempsInici);
}

function rebreTecla(tecla){
    console.log(tecla)
}

</script>

<template>
    <div>
        <KeyPad 
            :tecles="tecles"
            :bloquejat="!jugant"
            @premuda="rebreTecla"
        
        />
        <button @click="iniciarPartida"></button>
    </div>

</template>