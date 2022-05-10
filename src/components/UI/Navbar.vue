<template>
  <div class="navbar">
    <div @click="$router.push('/')">
      <h3 style="font-family: 'Days One'; color: #7E7E7E; cursor: pointer; text-align: center; margin-left: 5px">To Do List</h3>
    </div>
    <div class="navbar__btns">
      <button class="headerBtn" @click="$router.push('/posts')" style="margin-right:20px">Задачи</button>
      <button class="headerBtn" @click=outUser  style="margin-right:20px">Выйти</button>
<!--      <button class="headerBtn" @click="$router.push('/about')">Справка</button>-->
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    outUser() {
      this.$router.push('/')
      localStorage.clear()
    }
}

}
</script>

<style scoped>
.navbar {
  height: 50px;
  box-shadow: 0px 4px 50px rgba(139, 139, 139, 0.25);;
  display: flex;
  align-items: center;
  padding: 0 15px;
}

.navbar__btns {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-left: auto;
}

.headerBtn {
  padding: 10px 15px;
  border: none;
  cursor: pointer;
  border-radius: 15px;
  font-family: 'Days One';
  font-style: normal;
  text-align: center;
  color: #7E7E7E;
}
</style>