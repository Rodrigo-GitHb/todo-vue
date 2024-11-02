<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',

  tarefas: [
    {
      titulo:'Estudar ES6',
      finalizada: false,
    },
    {
      titulo:'Estudar SASS',
      finalizada: false,
    },
    {
      titulo:'Ir para a academia',
      finalizada: true,
    }
  ]
})

const getPendentes = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada === false)//Ou !tarefa.finalizada
}

const getFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada === true)//Ou tarefa.finalizada
}

const getFiltradas = () => {
  const { filtro}  = estado;

  switch (filtro) {
    case 'pendentes':
      return getPendentes();
    case 'finalizadas':
      return getFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastra = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }

  estado.tarefas.push(tarefaNova)
  estado.tarefaTemp = ''
}
</script>

<template>
  <div class="container">
  <Cabecalho :tarefas-pendentes="getPendentes().length" />
  <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastra"/>
  <ListaDeTarefas :tarefas="getFiltradas()"/>

  </div>
</template>


