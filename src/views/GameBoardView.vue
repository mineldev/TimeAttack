<script setup>
import KeyPad from '@/components/KeyPad.vue'
import ResultsTable from '@/components/ResultsTable.vue'
import { ref, computed } from 'vue'
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

const puntuacio = computed(() => {
    return resultats.value.reduce((total, resultat) => {
        return total + resultat.error
    }, 0)
})

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
    <main class="game-container">

        <h1>TIME ATTACK</h1>
        <p class="subtitle">Posa a prova la teva precisió</p>

        <div class="game-panel">

            <KeyPad 
                :tecles="tecles"
                :bloquejat="!jugant"
                :premudes="resultats.map(r => r.tecla)"
                @premuda="rebreTecla"
            />

            <button class="start-button" @click="iniciarPartida">
                Iniciar partida
            </button>

            <ResultsTable 
                v-if="!jugant && resultats.length === tecles.length"
                :resultats="resultats"
            />

            <div 
                v-if="!jugant && resultats.length === tecles.length"
                class="score"
            >
                <h2>Puntuació final: {{ puntuacio.toFixed(2) }}</h2>
            </div>

        </div>
    </main>
</template>

<style scoped>

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: #111827;
}

.game-container {
    min-height: 100vh;
    background: #111827;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-family: Arial, sans-serif;
}

h1 {
    font-size: 3rem;
    letter-spacing: 5px;
    margin-bottom: 5px;
}

.subtitle {
    color: #9ca3af;
    margin-bottom: 30px;
}

.game-panel {
    background: #1f2937;
    padding: 40px;
    border-radius: 15px;
    width: 80%;
    max-width: 700px;
    text-align: center;
}

.start-button {
    margin-top: 30px;
    padding: 12px 25px;
    background: #2563eb;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
}

.start-button:hover {
    background: #1d4ed8;
}

.score {
    margin-top: 25px;
    color: #38bdf8;
}

</style>