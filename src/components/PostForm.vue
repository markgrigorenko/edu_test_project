<template>
  <div class="app">
    <form @submit.prevent>
      <h3  style="font-family: 'Days One'; cursor: default; color: #7E7E7E; text-align: center">CОЗДАТЬ ЗАДАЧУ</h3>
      <my-input
          v-focus
          v-model="post.title"
          type="text"
          placeholder="Название"/>
      <my-input
          v-model="post.body"
          type="text"
          placeholder="Описание"/>
      <my-input
          v-model="post.date"
          type="text"
          placeholder="Дэдлайн"/>
      <my-button
          class="btn"
          @click="createPost"
          style="align-self: flex-end; margin-top: 15px"
      >Создать</my-button>
    </form>
  </div>
</template>
<script>

import MyInput from "@/components/UI/MyInput";
export default {
  components: {MyInput},
  props: {
    editedPost: [Object]
  },
  data () {
    return {
      post: {
        title: this.titleHandler(),
        body: this.bodyHandler(),
        date: this.dateHandler(),
      },
    }
  },

  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: '',
        date: '',
      }
    },

    titleHandler() {
      if (this.editedPost === '') {
        return ''
      }
      else {
        return this.editedPost.title
        this.editedPost = ''
      }
    },
    bodyHandler() {
      if (this.editedPost === '') {
        return ''
      }
      else {
        return this.editedPost.body
        this.editedPost = ''
      }
    },
    dateHandler() {
      if (this.editedPost === '') {
        return ''
      }
      else {
        return this.editedPost.date
        this.editedPost = ''
      }
    },
  },
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>