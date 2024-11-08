<script setup>
    const props = defineProps(['cadastraTarefa', 'tarefaTemp', 'trocarFiltro', 'editaTarefaTemp'])
</script>

<template>
    <form @submit.prevent="props.cadastraTarefa">
      <div class="row">
        <!-- Campo de input da tarefa -->
        <div class="col-12 col-md-8">
          <input 
            :value="props.tarefaTemp" 
            @change="props.editaTarefaTemp" 
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
            @change="props.trocarFiltro" 
            class="form-control w-100 text-center">
            <option value="todas">Todas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
</template>