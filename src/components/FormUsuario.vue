<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>

  <!--Formulário-->
  <div v-if="formOn">
    <form @submit.prevent="this.gravarUsuario()">
      <label for="id">ID</label>
      <input
        type="text"
        id="id"
        name="id"
        v-model="id"
        placeholder="Código da Categoria.."
        disabled
      />

      <label for="name">Nome</label>
      <input
        type="text"
        id="name"
        name="name"
        v-model="name"
        placeholder="O name aqui.."
      />

      <label for="senha">Senha</label>
      <input
        type="text"
        id="senha"
        name="senha"
        v-model="senha"
        placeholder="A senha aqui.."
      />

      <label for="nivel">Nível</label>
      <input
        type="text"
        id="nivel"
        name="nivel"
        v-model="nivel"
        placeholder="Nível aqui.."
        maxlength="1"
      />

      <input type="submit" value="Cadastrar" />
    </form>
  </div>

  <!--Botão para cadastrar novo usuário-->
  <div style="display: flex; justify-content: end">
    <!--Maneira de chamar o evento com o VUE-->
    <!--@.... ou v-on:...  as duas maneiras funcionam-->
    <button @click="this.mostrarForm(true)">Novo Usuário</button>
  </div>

  <!--Tabela de Usuários-->
  <table id="customers">
    <thead>
      <tr>
        <th>Id</th>
        <th>Nome</th>
        <th>Senha</th>
        <th>Nível</th>
        <th colspan="2">Ações</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="usu in usuarios" :key="usu.id">
        <td>{{ usu.id }}</td>
        <td>{{ usu.name }}</td>
        <td>{{ usu.senha }}</td>
        <td>{{ usu.nivel }}</td>
        <td><button @click="this.editarUsuario(usu.id)">Alterar</button></td>
        <td><button @click="this.excluirUsuraio(usu.id)">Excluir</button></td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";

export default {
  name: "FormUsuario",
  props: {
    msg: String,
  },
  data() {
    return {
      id: 0,
      name: "",
      senha: "",
      nivel: "",
      formOn: false,
      usuarios: [],
    };
  },
  methods: {
    mostrarForm(flag) {
      this.formOn = flag;
    },
    resetarForm() {
      this.id = 0;
      this.name = "";
      this.senha = "";
      this.nivel = "";
    },
    gravarUsuario() {
      let url = "http://localhost:8080/apis/usuario";
      let data = {
        id: this.id,
        name: this.name,
        senha: this.senha,
        nivel: this.nivel,
      };
      axios
        .post(url, data)
        .then((response) => {
          this.carregarTabela();
          this.resetarForm();
          this.mostrarForm(false);
          console.log("Mensagem do Backend: " + response);
        })
        .catch((error) => {
          alert("Erro ao gravar!! " + error);
        });
      this.mostrarForm(false);
    },
    editarUsuario(id) {
      this.mostrarForm(true);
      axios
        .get("http://localhost:8080/apis/usuario/" + id)
        .then((result) => {
          const usuario = result.data;
          this.id = usuario.id;
          this.name = usuario.name;
          this.senha = usuario.senha;
          this.nivel = usuario.nivel;
        })
        .catch((error) => {
          alert("Deu errado meu parcero!! " + error);
        });
    },
    excluirUsuario(id) {
      axios
        .delete("http://localhost:8080/apis/usuario/" + id)
        .then((result) => {
          //mostrar a tabela novamente após a remoção
          this.carregarTabela();
        })
        .catch((error) => {
          //retornar um erro pois não foi possível realizar a exclusão
          alert("Deu erro meu bacano!!");
        });
    },
    carregarTabela() {
      axios
        .get("http://localhost:8080/apis/usuario")
        .then((result) => {
          this.usuarios = result.data;
        })
        .catch((error) => {
          alert("Deu errado meu parcero!!" + error);
        });
    },
  },
  mounted() {
    this.carregarTabela();
  }
};
</script>

<style>
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
