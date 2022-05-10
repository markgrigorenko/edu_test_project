<template>
  <div class="post">
    <div>
<!--      <div> {{post.id}}</div>-->
      <div> выполнить к: {{post.leadTime}}</div>
      <div> <strong> {{post.title}} </strong> </div>
      <div> {{post.description}} </div>
    </div>

    <div class="post__btns">
<!--      <my-button
      @click="$router.push(`/posts/${post.id}`)"
      >Открыть</my-button>-->
      <my-button @click="$emit('update', post)"
      >Изменить</my-button>

      <my-button style="margin-left: 10px"
          @click="$emit('remove', post)"
      >✔</my-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
      post: {
        type: Object,
        required: true,
      }
    }
}
</script>

<style scoped>
.post {
  padding: 15px;
  margin-top: 15px;
  border: 5px solid #E4E4E4;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 15px;
}

.post__btns {
  display: flex;
  justify-content: space-between;
}
</style>