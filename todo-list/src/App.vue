<script setup>
import { reactive } from 'vue';

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
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastra">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descição da tarefa" class="form-control mb-3">
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
      </div>
      <div class="col-md-2">
        <select @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada === true}" class="ms-2" :for="tarefa.titulo">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
