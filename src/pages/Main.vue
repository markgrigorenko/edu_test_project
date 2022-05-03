<template>
  <div >
    <div class="auth">
      <h3 style="font-family: 'Days One'; color: #7E7E7E; cursor: default; text-align: center; margin-left: 5px"> ДОБРО ПОЖАЛОВАТЬ! </h3>
      <div class="authForm">
        <my-input v-model="login" placeholder="Логин"></my-input>
        <my-input v-model="password" placeholder="Пароль"></my-input>
        <my-button @click="authorization"  style="margin-top: 15px"> Войти </my-button>
      </div>
    </div>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MyButton from "@/components/UI/MyButton";
export default {
  components: {MyButton, MyInput},
  data() {
    return {
      login: '',
      password: ''
    }
  },
  methods: {
    authorization() {
      if (this.login === '' && this.password === '') {
        alert('ошибка')
        return
      }
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
</style>