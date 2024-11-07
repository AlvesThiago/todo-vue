<script setup>

import { reactive } from 'vue';

 const estado = reactive({
  filtro: 'todas',

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

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
        <h1>Minhas Tarefas</h1>
        <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form>
      <div class="row">
        <div class="col">
          <input type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button class="btn btn-primary" type="submit">Cadastrar</button>
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
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
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
