<template>
  <div class="about">
  <h1>Bem vindo {{ nome }}</h1>
  <label for="nome">Nome: </label>
  <input id="nome" type="text" v-model="nome" />
  <label for="senha">Senha: </label>
  <input id="senha" type="password" v-model="senha" />
  <button @click="cadastrar">Cadastrar</button>
  <p v-if="erro">{{ erro }}</p>
  <table>
  <thead>
  <td>Id</td>
  <td>Nome</td>
  </thead>
  <tr v-for="usuario in usuarios" :key="usuario.id">
  <td>{{ usuario.id }}</td>
  <td>{{ usuario.nome }}</td>
  </tr>
  </table>
  </div>
 </template>
 <script setup lang="ts">
 import { onMounted, ref } from 'vue';
 import axios from 'axios';
 const nome = ref("Nome");
 const senha = ref("123");
 const erro = ref();
 const usuarios = ref();
 async function atualizar() {
  try {
  usuarios.value = (await axios.get("usuario")).data;
  }
  catch(ex) {
  erro.value = (ex as Error).message;
  }
 }
 async function cadastrar() {
  try {
  await axios.post("usuario", 
  { 
  nome: nome.value,
  senha: senha.value
  });
  }
  catch(ex) {
  erro.value = (ex as Error).message;
}
 atualizar();
}
onMounted(() => {
 atualizar();
});
</script>
