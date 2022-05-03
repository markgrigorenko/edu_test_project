<template>
  <div v-if="posts.length > 0">
    <h4  style="font-family: 'Days One'; cursor: default; color: #7E7E7E;">{{posts[0].title}}</h4>
    <transition-group name="user-list">
      <post-item
          v-for="post in posts"
          :post="post"
          :key="post.id"
          @remove="$emit('remove', post)"
          @update="$emit('update', post)"
      />
    </transition-group>
  </div>
<!--  <h2 v-else>
    Список постов пуст
  </h2>-->

</template>

<script>
import PostItem from "@/components/PostItem";
export default {
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>
.user-list-item {
  display: inline-block;
  margin-right: 10px;
}
.user-list-enter-active,
.user-list-leave-active {
  transition: all 0.4s ease;
}
.user-list-enter-from,
.user-list-leave-to /* .list-leave-active до версии 2.1.8 */ {
  opacity: 0;
  transform: translateX(130px);
}

.user-list-move {
  transition: transform 0.4s ease;
}
</style>