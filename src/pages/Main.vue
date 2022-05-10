<template>
  <div >
    <div class="auth">
      <h3 style="font-family: 'Days One'; color: #7E7E7E; cursor: default; text-align: center; margin-left: 5px"> ДОБРО ПОЖАЛОВАТЬ! </h3>
      <div class="authForm">
        <my-input v-model="login" placeholder="Логин"></my-input>
        <my-input v-model="password" placeholder="Пароль"></my-input>
        <div class="buttons">
          <my-button @click="authorization"  style="margin-top: 15px"> Войти </my-button>
          <my-button @click="registration"  style="margin-top: 15px"> Зарегистрироваться </my-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MyButton from "@/components/UI/MyButton";
import axios from "axios";
export default {
  components: {MyButton, MyInput},
  data() {
    return {
      login: '',
      password: ''
    }
  },
  methods: {
    async authorization() {
      if (this.login === '' && this.password === '') {
        alert('Поля авторизации пустое')
        return
      }
      const data = await axios.post(
          'http://localhost:7028/graphql/',
          {query:`mutation {
    authUser (userInput :{
    username: "${this.login}"
    password: "${this.password}"
  }) {
    token
    error
    status
  }
}`})
      if (data.data.data.authUser.error === "User is not exist") {
        alert('Пользователь не найден. Зарегистриуйтесь, пожалуйста')
        return
      }
      if (data.data.data.authUser.error === "Password is not correct") {
        alert('Пароль неверен. Попробуйте ещё раз')
        return
      }
      localStorage.setItem("userToken", data.data.data.authUser.token)
      this.$router.push('/posts')
    },

    async registration() {
      if (this.login === '' && this.password === '') {
        alert('Поля регистрации пустые')
        return
      }
      const data = await axios.post(
          'http://localhost:7028/graphql/',
          {query:`mutation {
    registerUser (userInput :{
    username: "${this.login}"
    password: "${this.password}"
  }) {
    token
    error
    status
  }
}`})
      if (data.data.data.registerUser.error === "User with this username already exist") {
        alert('Пользователь с этим именем уже существует. Пожалуйста, выберете другое')
        return
      }
      localStorage.setItem("userToken", data.data.data.registerUser.token)
      this.$router.push('/posts')
    },
  }
}
</script>

<style scoped>
.authForm {
  width: 300px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

.auth {
  background-color: #FFFFFF;
  border-radius: 5px;
  padding-top: 75px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.buttons {
  width: 300px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
</style>