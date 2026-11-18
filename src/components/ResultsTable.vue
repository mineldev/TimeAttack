<script setup>
defineProps({
    resultats: Array
})
</script>

<template>
    <div class="results-container">
        <h2>Resultats de la partida</h2>

        <table>
            <thead>
                <tr>
                    <th>Tecla</th>
                    <th>Temps objectiu</th>
                    <th>Temps real</th>
                    <th>Error</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="resultat in resultats" :key="resultat.tecla">
                    <td class="key-cell">{{ resultat.tecla }}</td>
                    <td>{{ resultat.objectiu }}s</td>
                    <td>{{ resultat.tempsReal.toFixed(2) }}s</td>
                    <td class="error-cell">{{ resultat.error.toFixed(2) }}s</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style scoped>
.results-container {
    margin-top: 35px;
    width: 100%;
}

h2 {
    color: #60a5fa;
    margin-bottom: 20px;
}

table {
    width: 100%;
    border-collapse: collapse;
    color: white;
    background: #111827;
    border-radius: 10px;
    overflow: hidden;
}

th {
    background: #374151;
    color: #93c5fd;
    padding: 14px 10px;
    font-size: 14px;
}

td {
    padding: 14px 10px;
    border-bottom: 1px solid #374151;
    text-align: center;
}

tr:last-child td {
    border-bottom: none;
}

tbody tr:hover {
    background: #263449;
}

.key-cell {
    font-weight: bold;
    color: #60a5fa;
}

.error-cell {
    color: #4ade80;
    font-weight: bold;
}
</style>