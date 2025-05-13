<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>

  <!--Formulário-->
  <div v-if="formOn">
    <form @submit.prevent="this.gravarCategoria()">
      <label for="id">ID</label>
      <input
        type="text"
        id="id"
        name="id"
        placeholder="Código da Categoria.."
      />

      <label for="nome">Nome</label>
      <input type="text" id="nome" name="nome" placeholder="O nome aqui.." />

      <input type="submit" value="Cadastrar" />
    </form>
  </div>

  <!--Botão para cadastrar nova categoria-->
  <div style="display: flex; justify-content: end">
    <!--Maneira de chamar o evento com o VUE-->
    <!--@.... ou v-on:...  as duas maneiras funcionam-->
    <button @click="this.mostrarForm(true)">Nova Categoria</button>
  </div>

  <!--Tabela-->
  <table id="customers">
    <thead>
      <tr>
        <th>Id</th>
        <th>Nome</th>
        <th colspan="2">Ações</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cat in categorias" :key="cat.id">
        <td>{{ cat.id }}</td>
        <td>{{ cat.nome }}</td>
        <td><button @click="this.editarCategoria(cat.id)">Alterar</button></td>
        <td><button @click="this.excluirCategoria(cat.id)">Excluir</button></td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from 'axios';

export default {
  name: "FormCategoria",
  props: {
    msg: String,
  },
  data() {
    return {
      id: 0,
      nome: "",
      formOn: false,
      categorias:[]
    };
  },
  methods: {
    mostrarForm(flag) {
      this.formOn = flag;
    },
    gravarCategoria() {
      alert("gravando!!");
      this.mostrarForm(false);
    },
    editarCategoria(id) {
      alert("editando!!");
    },
    excluirCategoria(id) {
      alert("excluindo!!");
    },
    carregarTabela() {
      axios.get("http://localhost:8080/apis/categoria")
      .then(result=>{
        this.categorias = result.data;
      })
      .catch(error=>{
        alert("Deu errado meu parcero!!" + error);
      })
    },
  },
  mounted(){
    this.carregarTabela();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*Estilo para o formulário*/
input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type="submit"] {
  width: 100%;
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}

/*estilo para a tabela*/
#customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#customers td,
#customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even) {
  background-color: #f2f2f2;
}

#customers tr:hover {
  background-color: #ddd;
}

#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
}
</style>
