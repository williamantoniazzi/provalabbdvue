<template>
    <div class="consulta"></div>
    <div>
        <p>
            <label>Descrição: </label>
            <input type="text" v-model="descricao" />
        </p>
        <p>
            <label>Data/hora início: </label>
            <input type="datetime-local" v-model="dataHoraInicio" />
        </p>
        <p>
            <label>Duração: </label>
            <input type="text" v-model="duracao" />
        </p>
        <button @click="cadastrar">Cadastrar</button>
        <button @click="atualizar">Atualizar</button>
        <table>
            <thead>
                <td>Descrição</td>
                <td>Data e Hora</td>
                <td>Duração</td>
            </thead>
            <tr v-for="consulta in consultas" :key="consulta.descricao">
                <td>{{ consulta.descricao }}</td>
                <td>{{ consulta.dataHoraInicio }}</td>
                <td>{{ consulta.duracao }} min</td>
            </tr>
        </table>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

const descricao = ref("");
const dataHoraInicio = ref("");
const duracao = ref("");

const consultas = ref([{descricao: "", dataHoraInicio: "", duracao: ""}]);

async function atualizar() {
    consultas.value = (await axios.get('consulta')).data;
}

async function cadastrar() {
    await axios.post('consulta',
        {
            descricao: descricao.value,
            dataHoraInicio: dataHoraInicio.value,
            duracao: duracao.value,
        });
    atualizar();
    descricao.value = "";
    dataHoraInicio.value = "";
    duracao.value = "";
}

onMounted(() => {
    atualizar();
});

</script>