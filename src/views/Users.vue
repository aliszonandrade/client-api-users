<template>
  <div>
    <h1>Painel adm</h1>
    <br />
    <div class="columns is-centered">
      <table border="1px;">
        <th>Nome</th>
        <th>E-mail</th>
        <th>Cargo</th>
        <th>Ações</th>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ processRole(user.role) }}</td>
          <td>
            <router-link :to="{name: 'Edit', params:{id: user.id}}"><button class="button is-success">Editar</button></router-link>
            <button class="button is-danger" @click="showOrHideModal(user.id)">
              Deletar
            </button>
          </td>
        </tr>
      </table>
    </div>
    <div :class="{ modal: true, 'is-active': showModal }">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Deletar usuário</p>
          <button
            class="delete"
            @click="showOrHideModal()"
            aria-label="close"
          ></button>
        </header>
        <section class="modal-card-body">
          Você quer realmente deletar este usuário?
        </section>
        <footer class="modal-card-foot columns is-centered">
          <button class="button is-success" @click="deleteUser()">
            Confirmar
          </button>
          <button class="button is-danger" @click="showOrHideModal()">
            Cancelar
          </button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created() {
    this.getUsers();
  },
  data() {
    return {
      users: [],
      showModal: false,
      deleteUserId: -1,
    };
  },
  methods: {
    getUsers() {
      var req = {
        headers: {
          authorization: "Bearer " + localStorage.getItem("token"),
        },
      };

      axios
        .get("http://localhost:8081/user", req)
        .then((res) => {
          console.log(res.data);
          this.users = res.data;
        })
        .catch((error) => {
          console.log(error.response);
        });
    },

    processRole(value) {
      if (value == 0) return "Usuário";
      else if (value == 1) return "Admin";
    },

    showOrHideModal(id) {
      this.showModal = !this.showModal;
      this.deleteUserId = id;
    },
    
    deleteUser() {
      var req = {
        headers: {
          authorization: "Bearer " + localStorage.getItem("token"),
        },
      };

      axios
        .delete("http://localhost:8081/user/" + this.deleteUserId, req)
        .then((res) => {
          console.log(res);
          this.showOrHideModal();
          this.getUsers();
        })
        .catch((error) => {
          console.log(error);
          this.showOrHideModal();
        });
    },
  },
};
</script>

<style scoped>
</style>