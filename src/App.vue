<script setup>
import { ref, computed } from 'vue'
import TasckChild from './components/TasckChild.vue';
const tarefas = ref([
  { id: 1, desc: 'Prova Geografia', status: 'pendente' },
  { id: 2, desc: 'Prova História', status: 'concluida' },
  { id: 3, desc: 'Trabalho DevWeb', status: 'pendente' }
]);

const novaTarefa = ref('')
const posicaoAlterar = ref(-1)
const filtro = ref('')

const tarefasPendentes = computed(() => {
  return tarefas.value.filter(t => t.status == 'pendente').length;
})

const tarefasConcluidas = computed(() => {
  return tarefas.value.filter(t => t.status == 'concluida').length;
})

const tarefasFiltradas = computed(() => {
  if (filtro.value.trim() == '') {
    return tarefas.value;
  }
  else {
    return tarefas.value.filter(t => t.desc.includes(filtro.value))
  }
})

function ordenar() {
  tarefas.value.sort((a, b) => a.desc.localeCompare(b.desc, 'pt-BR'))
}

function addTarefa() {
  if (posicaoAlterar.value == -1) {
    let maiorID = Math.max(...tarefas.value.map(item => item.id));
    tarefas.value.push({
      id: maiorID + 1,
      desc: novaTarefa.value,
      status: 'pendente'
    });
  }
  else {
    tarefas.value[posicaoAlterar.value].desc = novaTarefa.value
    posicaoAlterar.value = -1
  }
  novaTarefa.value = '';
}

function deleteTarefa(idTarefa) {
  const posicao = tarefas.value.findIndex(t => t.id === idTarefa                                                                                                                                                                                                                                                                 .id);
  tarefas.value.splice(posicao, 1);
}

function editTarefa(item) {
  posicaoAlterar.value = tarefas.value.findIndex(t => t.id === item.id);
  novaTarefa.value = tarefas.value[posicaoAlterar.value].desc;
}
function marcarConcluida(id) {
  const posicao =
    tarefas.value.findIndex(t => t.id == id);
  if (tarefas.value[posicao].status == 'concluida') {
    tarefas.value[posicao].status = 'pendente'
  }
  else {
    tarefas.value[posicao].status = 'concluida'
  }
}
</script>

<template>
  <div class="container">
    <input type="text" v-model="novaTarefa">
    <button @click="addTarefa">Adicionar</button>
    <ul>
      <TasckChild v-for="tarefa in tarefas" :key="tarefa.id"
      :id="tarefa.id" :descricao="tarefa.desc" :status="tarefa.status"
      @editar="deleteTarefa"
      >

      </TasckChild>
    </ul>
    <div>
      <input type="text" placeholder="Filtrar Tarefa..." v-model="filtro">
      <button @click.prevent="ordenar()">Ordenar</button>
    </div>
    <div>
      <span>Pendentes: {{ tarefasPendentes }}</span>
      <span>Concluídas: {{ tarefasConcluidas }}</span>
    </div>
  </div>
</template>

<style scoped>
li {
  cursor: pointer;
}

.concluida {
  text-decoration: line-through;
}
</style>
