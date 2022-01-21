<template>
    <div>
        <h2>Login</h2>
        <hr>
        <div class="columns is-centered">
            <div class="column is-half">
                <div class="notification is-danger" v-if="error != undefined">
                    <p>{{error}}</p>
                </div>
                <p>E-mail</p>
                <input type="text" placeholder="E-mail" class="input"  v-model="email">
                <p>Senha</p>
                <input type="password" placeholder="******" class="input"  v-model="password">
                <br><br>
                <button class="button is-success" @click="login()">Logar</button>
            </div>
        </div>
        
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data(){
        return {
            password: '',
            email: '',
            error: undefined
        }
    },
    methods: {
        login(){
            axios.post("http://localhost:8081/login",{
                password: this.password,
                email: this.email            
            }).then(res => {
                console.log(res);
                localStorage.setItem('token', res.data.token);
                this.$router.push({name: 'Users'});

            }).catch(error => {
                console.log(error)
                var msgErro = error.response.data.error;
                this.error = msgErro;
            })
        }
    }
    
}
</script>

<style scoped>

</style>