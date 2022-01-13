<template>
    <div>
        <h2>Registro de usuário!</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <div class="notification is-danger" v-if="error != undefined">
                    <p>{{error}}</p>
                </div>
                <p>Nome</p>
                <input type="text" placeholder="Nome" class="input" v-model="name">
                <p>E-mail</p>
                <input type="text" placeholder="E-mail" class="input"  v-model="email">
                <p>Senha</p>
                <input type="password" placeholder="******" class="input"  v-model="password">
                <br><br>
                <button class="button is-success" @click="register()">Cadastrar</button>
            </div>
        </div>
        
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data(){
        return {
            name: '',
            password: '',
            email: '',
            error: undefined
        }
    },
    methods: {
        register(){
            axios.post("http://localhost:8081/user",{
                name: this.name,
                password: this.password,
                email: this.email            
            }).then(res => {
                console.log(res);
                this.$router.push({name: 'Home'});

            }).catch(error => {
                console.log(error)
                var msgErro = error.response.data.err;
                this.error = msgErro;
            })
        }
    }
    
}
</script>

<style scoped>

</style>