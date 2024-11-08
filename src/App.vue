<script setup>
  import { reactive, onMounted, watch } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: []
  });

  // Função para carregar as tarefas do localStorage
  const carregarTarefas = () => {
    const tarefasSalvas = localStorage.getItem('tarefas');
    if (tarefasSalvas) {
      estado.tarefas = JSON.parse(tarefasSalvas);
    }
  };

  // Função para salvar as tarefas no localStorage
  const salvarTarefas = () => {
    localStorage.setItem('tarefas', JSON.stringify(estado.tarefas));
  };

  // Carregar tarefas do localStorage quando o componente for montado
  onMounted(() => {
    carregarTarefas();
  });

  // Assistir alterações nas tarefas e salvar no localStorage
  watch(() => estado.tarefas, salvarTarefas, { deep: true });

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  };

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  };

  const getTarefasFiltradas = () => {
    const { filtro } = estado;
    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  };

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false
    };
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = ''; // Limpa o campo de input após cadastrar
  };
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario 
      :trocar-filtro="evento => estado.filtro = evento.target.value" 
      :tarefa-temp="estado.tarefaTemp" 
      :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value"  
      :cadastra-tarefa="cadastraTarefa" 
    />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>

