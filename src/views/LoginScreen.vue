<template>
  <div class="container_login_screen">
    <div class="logo_container">
      <div>
        <img class="logo" src="../assets/images/logo.png" alt="logo" />
        <h1 class="title_logo">FACILITA<span>TASKS</span></h1>
        <h3>Gestão de tarefas pessoais</h3>
      </div>
    </div>
    <div class="login_container">
      <div class="login_box">
        <h1 class="title_login">Entre com seus dados de acesso.</h1>
        <form @submit.prevent="handleLogin">
          <div class="input_login">
            <span>Nome de usuário ou e-mail:</span>
            <input type="text" v-model="username" />
          </div>
          <div class="input_login">
            <span>Senha:</span>
            <input type="password" v-model="password" />
          </div>
          <button class="button_login" type="submit">Entrar</button>
        </form>
        <span class="forgot_password">Esqueci minha senha </span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";

const username = ref("");
const password = ref("");

const router = useRouter();

const handleLogin = () => {
  if (username.value === "admin" && password.value === "password") {
    router.push("/dashboard");
  } else {
    alert("Credenciais inválidas. Por favor, tente novamente.");
  }
};
</script>

<style lang="stylus" scoped>
.container_login_screen
  display: flex
  align-items: center
  background-color: #ffffff
  width: 100vw
  height: 100vh

.logo_container
  display: flex
  width: 45%
  height: 100%
  color: #ffffff
  align-items: center
  justify-content: center
  background-color: #2693ff

  .title_logo
    font-size: 35px
    font-weight: bold
    margin: 10px 0

  span
    opacity: 90%

.logo
  width: 100px
  height: 100px
  border-radius: 15%

.login_container
  display: flex
  width: 55%
  justify-content: center

.login_box
  display: flex
  flex-direction: column

.input_login
  display: flex
  flex-direction: column

  input
    width: 320px
    height: 40px
    border-radius: 7px
    border: 2px solid rgba(0, 0, 0, 0.3)
    margin: 5px 0 10px 0
    font-size: 15px
    transition: border-color 0.3s ease
    box-sizing: border-box

    &:focus
      border-color: #2693ff
      outline: none

  span
    font-size: 13px

.title_login
  margin-bottom: 30px
  font-size: 16px
  font-weight: 600

.button_login
  color: white
  font-size: 15px
  margin-top: 10px
  width: 320px
  height: 40px
  border: none
  border-radius: 7px
  background: #1ad18f
  cursor: pointer

.forgot_password
  margin-top: 30px
  text-align: left
  width: 100%
  color: #5caeff
  text-decoration: underline
  cursor: pointer
</style>
