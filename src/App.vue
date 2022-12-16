<template>
  <div>
    <h1>Lista de tarefas</h1>
    <form @submit.prevent="adicionarTarefa">
      <input v-model="tarefa.nome"
        autocomplete="off"
        required
        id="myInput"
        placeholder="Acrescentar tarefa"
      />
      <button v-if="isCadastro && isEdicao !== true && isExclusao !== true" style="background-color: #004f9f; color: #fff">Cadastrar</button>
      <button v-if="isEdicao" style="margin-left: 20px; background-color: #004f9f; color: #fff" @click="confirmarEdicaoTarefa()">Concluir edição</button>
      <button v-if="isExclusao" style="margin-left: 20px; background-color: #ff5252; color: #fff" @click="confirmarExclusao()">Confirmar exclusão</button>
      <button v-if="isExclusao" style="margin-left: 20px; background-color: #004f9f; color: #fff" @click="cancelarExclusao()">Cancelar exclusão</button>
    </form>
    <h3>Suas tarefas do dia !</h3>
    <table>
      <li v-for="(tarefa, key) in tarefas" :key="key">
         {{ tarefa.id }} {{ tarefa.nome }}
        <button @click="editarTarefa(tarefa, key)" style="margin-left: 30px; background-color: #004f9f; color: #fff">Editar</button>
        <button @click="excluirTarefa(key)" style="margin-left: 30px; background-color: #ff5252; color: #fff">Excluir</button>
        <hr>
      </li>
    </table>
  </div>
</template>

<script>

export default {
  data() {
    return {
      id: 0,
      tarefa: {id: this.id++, nome: ''},
      tarefas: [
        {id: 1, nome: 'Lavar'},
        {id: 2, nome: 'Limpar'},
        {id: 3, nome: 'Secar'}
      ],
      indiceClicado: null,
      isCadastro: true,
      isEdicao: false,
      isExclusao: false
    }
  },
  methods: {
    adicionarTarefa() {
      this.tarefa = {id: this.tarefas.length + 1, nome: this.tarefa.nome}
      this.tarefas.push(this.tarefa);
      this.tarefa = {id: this.id++, nome: ''}
    },
    editarTarefa(tarefa, key) {
      this.isEdicao = true;
      this.tarefa = Object.assign({}, tarefa);
      this.indiceClicado = key;
    },
    confirmarEdicaoTarefa() {
      this.tarefas[this.indiceClicado].nome = this.tarefa.nome;
      this.indiceClicado = null;
      this.tarefa = {id: this.id++, nome: ''};
      this.isEdicao = false;
    },
    excluirTarefa(key) {
      this.isExclusao = true;
      this.indiceClicado = key;
    },
    confirmarExclusao() {
      this.tarefas.splice(this.indiceClicado, 1);
      this.isExclusao = false;
    },
    cancelarExclusao() {
      this.tarefa = {id: this.id++, nome: ''};
      this.indiceClicado = null;
      this.isExclusao = false;
    }
  }
}
</script>