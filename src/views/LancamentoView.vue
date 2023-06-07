<template>
    <div class="facalancamento"></div>
    <div>
        <p>
            <label>Justificativa: </label>
            <input type="text" v-model="justificativa" />
        </p>
        <p>
            <label>Data/hora início: </label>
            <input type="datetime-local" v-model="dataHoraInicio" />
        </p>
        <p>
            <label>Data/hora fim: </label>
            <input type="datetime-local" v-model="dataHoraFim" />
        </p>
        <p>
            <label>Tipo: </label>
            <input type="text" v-model="tipo" />
        </p>
        <button @click="cadastrar">Cadastrar</button>
        <table>
            <thead>
                <td>Justificativa</td>
                <td>Data/Hora Início</td>
                <td>Data/Hora Fim</td>
                <td>Tipo</td>
            </thead>
            <tr v-for="facalancamento in facalancamentos" :key="facalancamento.justificativa">
                <td>{{ facalancamento.justificativa }}</td>
                <td>{{ facalancamento.dataHoraInicio }}</td>
                <td>{{ facalancamento.dataHoraFim }}</td>
                <td v-if="facalancamento.tipo === 'N'">Normal</td>
                <td v-else-if="facalancamento.tipo === 'E'">Extra</td>
            </tr>
        </table>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

const justificativa = ref("");
const dataHoraInicio = ref("");
const dataHoraFim = ref("");
const tipo = ref("");

const facalancamentos = ref([{justificativa: "", dataHoraInicio: "", dataHoraFim: "", tipo: "N"}]);

async function atualizar() {
    facalancamentos.value = (await axios.get('facalancamento')).data;
}

async function cadastrar() {
    await axios.post('facalancamento',
        {
            justificativa: justificativa.value,
            dataHoraInicio: dataHoraInicio.value,
            dataHoraFim: dataHoraFim.value,
            tipo: tipo.value
        });
    atualizar();
    justificativa.value = "";
    dataHoraInicio.value = "";
    dataHoraFim.value = "";
    tipo: tipo.value = "";
}

onMounted(() => {
    atualizar();
});
</script>