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
          v-model="post.description"
          type="text"
          placeholder="Описание"/>
      <my-input
          v-model="post.leadTime"
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
        description: this.bodyHandler(),
        leadTime: this.dateHandler(),
        iD: Date.now()
      },
    }
  },

  methods: {
    createPost() {
      this.$emit('create', this.post)
      this.post = {
        title: '',
        iD: '',
        description: '',
        leadTime: '',
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
        return this.editedPost.description
        this.editedPost = ''
      }
    },
    dateHandler() {
      if (this.editedPost === '') {
        return ''
      }
      else {
        return this.editedPost.leadTime
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