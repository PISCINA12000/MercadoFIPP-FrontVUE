<template>
  <!-- <FormCategoria msg="Cadastro de Categorias"/> -->
  <FormUsuario msg="Cadastro de Usuários"/>
</template>

<script>
  import FormCategoria from './components/FormCategoria.vue';
  import FormUsuario from './components/FormUsuario.vue';

  export default {
    name: 'App',
    components: {
      FormUsuario
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
