<script setup>
import { useGameStore } from '@/stores/game'

const store = useGameStore()
</script>

<template>
    <div class="scoreboard">
        <h2 class="section-title">HISTORIAL DE PARTIDES</h2>

        <p v-if="store.partides.length === 0">
            Encara no hi ha partides guardades.
        </p>

        <div v-else>
            <table class="score-table">
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

        <button class="delete-button" @click="store.esborrarPartides()">
            Esborrar historial
        </button>

        <h2 class="section-title">CLASSIFICACIÓ GLOBAL</h2>

    <p v-if="store.classificacio.length === 0">
        Encara no hi ha resultats a la classificació.
    </p>

    <div v-else>
        <table class="score-table">
            <thead>
                <tr>
                    <th>Jugador</th>
                    <th>Error total</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="(resultat, index) in store.classificacio" :key="index">
                    <td>{{ resultat.nom }}</td>
                    <td>{{ resultat.errorTotal }} s</td>
                </tr>
            </tbody>
        </table>
    </div>

    </div>
</template>

<style scoped>

.scoreboard {
    margin-top: 30px;
    width: 100%;
}

.section-title {
    color: #38bdf8;
    font-size: 1.2rem;
    letter-spacing: 2px;
    margin: 30px 0 15px;
}

.score-table {
    width: 100%;
    border-collapse: collapse;
    background: #111827;
    border-radius: 10px;
    overflow: hidden;
    margin-bottom: 15px;
}

.score-table th {
    background: #374151;
    color: #38bdf8;
    padding: 12px;
    text-align: center;
}

.score-table td {
    padding: 12px;
    border-bottom: 1px solid #374151;
    color: #e5e7eb;
}

.score-table tr:last-child td {
    border-bottom: none;
}

.score-table tbody tr:hover {
    background: #273449;
}

.delete-button {
    margin-top: 10px;
    padding: 10px 18px;
    background: #991b1b;
    color: white;
    border: none;
    border-radius: 7px;
    cursor: pointer;
    transition: background 0.2s;
}

.delete-button:hover {
    background: #dc2626;
}

.scoreboard p {
    color: #9ca3af;
    margin: 15px 0;
}

</style>