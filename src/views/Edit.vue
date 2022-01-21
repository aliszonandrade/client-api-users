<template>
  <div>
    <h2>Edição de usuário</h2>
    <hr />
    <div class="columns is-centered">
      <div class="column is-half">
        <div class="notification is-danger" v-if="error != undefined">
          <p>{{ error }}</p>
        </div>
        <p>Nome</p>
        <input type="text" placeholder="Nome" class="input" v-model="name" />
        <p>E-mail</p>
        <input type="text" placeholder="E-mail" class="input" v-model="email" />
        <br /><br />
        <button class="button is-success" @click="update()">Salvar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created() {
    var req = {
      headers: {
        authorization: "Bearer " + localStorage.getItem("token"),
      },
    };

    axios
      .get("http://localhost:8081/user/" + this.$route.params.id, req)
      .then((res) => {
        this.name = res.data.name;
        this.email = res.data.email;
        this.id = res.data.id;
      })
      .catch((error) => {
        console.log(error);
        this.$router.push({ name: "Users" });
      });
  },
  data() {
    return {
      name: "",
      password: "",
      email: "",
      id: -1,
      error: undefined,
    };
  },
  methods: {
    update() {
      var req = {
        headers: {
          authorization: "Bearer " + localStorage.getItem("token"),
        },
      };

      axios
        .put(
          "http://localhost:8081/user",
          {
            name: this.name,
            email: this.email,
            id: this.id,
          },
          req
        )
        .then((res) => {
          console.log(res);
          this.$router.push({ name: "Users" });
        })
        .catch((error) => {
          console.log(error);
          var msgErro = error.response.data.err;
          this.error = msgErro;
        });
    },
  },
};
</script>

<style scoped>
</style>