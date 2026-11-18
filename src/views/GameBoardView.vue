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
    const tempsActual = performance.now();
    const tempsReal = (tempsActual - tempsInici) / 1000;

    const objectiu = tempsObjectiu[tecla];
    const error = Math.abs(tempsReal - objectiu);

    resultats.value.push({
        tecla: tecla,
        objectiu: objectiu,
        tempsReal: tempsReal,
        error: error
    });

    console.log(resultats.value);

    if(resultats.value.length === tecles.length){
        jugant.value = false;
    }
}

</script>

<template>
    <div>
        <KeyPad 
            :tecles="tecles"
            :bloquejat="!jugant"
            :premudes="resultats.map(r => r.tecla)"
            @premuda="rebreTecla"
        
        />
        <button @click="iniciarPartida"></button>
    </div>

</template>