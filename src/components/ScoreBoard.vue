<script setup>
import { useGameStore } from '@/stores/game'

const store = useGameStore()
</script>

<template>
    <div>
        <h2>HISTORIAL DE PARTIDES</h2>

        <p v-if="store.partides.length === 0">
            Encara no hi ha partides guardades.
        </p>

        <div v-else>
            <table>
                <thead>
                    <tr>
                        <th>Partida</th>
                        <th>Jugador</th>
                        <th>Error total</th>
                    </tr>
                </thead>

                <tbody>
                    <tr v-for="partida in store.partides" :key="partida.id">
                        <td>{{ partida.id }}</td>
                        <td>{{ partida.nom }}</td>
                        <td>{{ partida.errorTotal }} s</td>
                    </tr>
                </tbody>
            </table>
        </div>

        <button @click="store.esborrarPartides()">
            Esborrar historial
        </button>
    </div>
</template>