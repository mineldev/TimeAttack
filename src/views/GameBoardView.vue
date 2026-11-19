<script setup>
import { ref, computed } from 'vue'
import { useGameStore } from '@/stores/game'

import { onMounted, onUnmounted } from 'vue'
import { socket } from '@/services/socket'


import KeyPad from '@/components/KeyPad.vue'
import ResultsTable from '@/components/ResultsTable.vue'
import ScoreBoard from '@/components/ScoreBoard.vue'

console.log('GAMEBOARD CARGADO')

const store = useGameStore()
console.log('Nombre guardado:', store.nomJugador)

const nom = ref('')

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

function connectat() {
    console.log('Connectat al servidor!', socket.id)
}

function rebreClassificacio(dades) {
    console.log('Classificació rebuda:', dades)
    store.actualitzarClassificacio(dades)
}

onMounted(() => {
    socket.on('connect', connectat)
    socket.on('classificacio', rebreClassificacio)
    
    socket.connect()
})

onUnmounted(() => {
    socket.off('connect', connectat)
    socket.on('classificacio', rebreClassificacio)

    socket.disconnect()
})

function entrar() {
    if (nom.value.trim() === '') return

    store.guardarNom(nom.value.trim())
}

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

    if (resultats.value.length === tecles.length) {
        jugant.value = false

        const errorTotal = resultats.value.reduce((total, r) => {
            return total + r.error
        }, 0)

        const errorFinal = Number(errorTotal.toFixed(2))

        store.guardarPartida(errorFinal)

        socket.emit('resultat', {
            nom: store.nomJugador,
            errorTotal: errorFinal
        })
    }
}

</script>

<template>
    <main class="game-container">

        <h1>TIME ATTACK</h1>
        <p class="subtitle">Posa a prova la teva precisió</p>
        
    <div v-if="!store.nomJugador" class="welcome-panel">
        <h2>Benvingut a TIME ATTACK!</h2>

        <p>Introdueix el teu nom per començar a jugar</p>

        <input
            v-model="nom"
            placeholder="El teu nom..."
            @keyup.enter="entrar"
        />

        <button @click="entrar">
            Entrar al joc
        </button>
    </div>

    <div v-else class="game-panel">

        <h2>Hola, {{ store.nomJugador }}!</h2>
        
        <ScoreBoard />
        
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

.game-container {
    min-height: 100vh;
    width: 100%;
    padding: 40px 20px;
    background: #111827;
    color: white;

    display: flex;
    flex-direction: column;
    align-items: center;

    font-family: Arial, sans-serif;
}

h1 {
    font-size: 3rem;
    letter-spacing: 5px;
    margin-bottom: 5px;
    color: #38bdf8;
    text-shadow: 0 0 20px rgba(56, 189, 248, 0.3);
}

.subtitle {
    color: #9ca3af;
    margin-bottom: 30px;
}

.game-panel {
    background: #1f2937;
    padding: 35px;
    border: 1px solid #374151;
    border-radius: 18px;
    width: 100%;
    max-width: 900px;
    text-align: center;
    box-shadow: 0 10px 35px rgba(0, 0, 0, 0.3);
}

.game-panel h2 {
    margin-bottom: 25px;
    color: #e5e7eb;
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
    font-weight: bold;
    transition: background 0.2s, transform 0.2s;
}

.start-button:hover {
    background: #1d4ed8;
    transform: translateY(-2px);
}

.score {
    margin-top: 25px;
    color: #38bdf8;
}


.welcome-panel {
    background: #1f2937;
    padding: 45px 35px;
    border: 1px solid #374151;
    border-radius: 18px;
    width: 100%;
    max-width: 450px;
    text-align: center;
    box-shadow: 0 10px 35px rgba(0, 0, 0, 0.3);
}

.welcome-panel h2 {
    font-size: 1.5rem;
    margin-bottom: 12px;
}

.welcome-panel p {
    color: #9ca3af;
    margin-bottom: 25px;
}

.welcome-panel input {
    width: 100%;
    padding: 13px;
    background: #111827;
    border: 1px solid #4b5563;
    border-radius: 8px;
    color: white;
    font-size: 16px;
    outline: none;
    margin-bottom: 15px;
}

.welcome-panel input:focus {
    border-color: #38bdf8;
    box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.15);
}

.welcome-panel button {
    width: 100%;
    padding: 13px;
    background: #2563eb;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.2s, transform 0.2s;
}

.welcome-panel button:hover {
    background: #1d4ed8;
    transform: translateY(-2px);
}


</style>