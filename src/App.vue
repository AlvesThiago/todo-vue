<script setup>

import { reactive } from 'vue';

 const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [

    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },

    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },

    {
      titulo: 'Academia',
      finalizada: true,
    }

  ]
 })

 const getTarefasPendentes = () => {
   return estado.tarefas.filter(tarefa => !tarefa.finalizada)
 }

 const getTarefasFinalizadas = () => {
   return estado.tarefas.filter(tarefa => tarefa.finalizada)
 }


 const getTarefasFiltradas = () => {
  const {filtro} = estado;

  switch (filtro){
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();

    default:
      return estado.tarefas;
  }

 }

 const cadastraTarefa = (e) => {
  e.preventDefault()
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }

  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
 }

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>

    <form @submit="cadastraTarefa">
      <div class="row">
        <!-- Campo de input da tarefa -->
        <div class="col-12 col-md-8">
          <input 
            :value="estado.tarefaTemp" 
            @change="evento => estado.tarefaTemp = evento.target.value" 
            required 
            type="text" 
            placeholder="Digite aqui a descrição da tarefa" 
            class="form-control">
        </div>

        <!-- Botão de cadastrar -->
        <div class="col-12 col-md-2 mt-2 mt-md-0">
          <button class="btn btn-primary w-100" type="submit">Cadastrar</button>
        </div>

        <!-- Filtro de tarefas -->
        <div class="col-12 col-md-2 mt-2 mt-md-0">
          <select 
            @change="evento => estado.filtro = evento.target.value" 
            class="form-control w-100 text-center">
            <option value="todas">Todas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    <!-- Lista de tarefas -->
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input 
          @change="evento => tarefa.finalizada = evento.target.checked" 
          :checked="tarefa.finalizada" 
          :id="tarefa.titulo" 
          type="checkbox">
        <label :class="{done: tarefa.finalizada === true}" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>


<style scoped>

  .done{
    text-decoration: line-through;
  }


</style>
