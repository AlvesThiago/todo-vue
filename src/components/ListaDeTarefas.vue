<script setup>
  const props = defineProps(['tarefas']);

  const excluirTarefa = (index) => {
    // Remove a tarefa pelo índice
    props.tarefas.splice(index, 1);
  };
</script>

<template>
  <!-- Lista de tarefas -->
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="(tarefa, index) in props.tarefas" :key="tarefa.titulo">
      <input 
        @change="evento => tarefa.finalizada = evento.target.checked" 
        :checked="tarefa.finalizada" 
        :id="tarefa.titulo" 
        type="checkbox">
      <label :class="{done: tarefa.finalizada === true}" class="ms-3" :for="tarefa.titulo">
        {{ tarefa.titulo }}
      </label>
      
      <!-- Botão de excluir -->
      <button 
        class="btn btn-danger btn-sm float-end ms-3" 
        @click="excluirTarefa(index)">
        Excluir
      </button>
    </li>
  </ul>
</template>



<style scoped>

  .done{
    text-decoration: line-through;
  }

</style>