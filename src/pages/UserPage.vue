<template>
  <div style="background-color: #FFFFFF; border-radius: 5px; padding-top: 8px">
    <h3 style="font-family: 'Days One'; cursor: default; color: #7E7E7E; padding-left: 13px"> Мои задачи </h3>
    <my-input
        v-model="searchQuery"
        placeholder="Поиск..."/>
    <div class="app__btns">
      <my-button
          @click="showDialog">
        Создать пост </my-button>
      <my-select
          @click="sortedAndSearchedPosts"
          v-model="selectedSort"
          :options="sortOptions"/>
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form :editedPost=this.editedPost
          @create="createPost"
      />
    </my-dialog>

    <post-list
        v-for="group in dataGroup"
        :posts="group"
        @remove="removePost"
        @update="updatePost"
        v-if="!isPostsLoading"
    />

    <div v-else>Идёт загрузка...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--    <div class="page__wrapper">
          <div v-for="pageNumber in totalPages"
               :key="pageNumber"
               class="page"
               :class="{
                 'current-page': page === pageNumber
               }"
               @click="changePage(pageNumber)"

          > {{pageNumber}}</div>
        </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyDialog from "@/components/UI/MyDialog";
import axios from 'axios'
import MyButton from "@/components/UI/MyButton";
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";

export default {
  components: {
    MyInput,
    MySelect,
    MyButton,
    MyDialog,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dataGroup: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      editedPost: '',

      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'Название'},
        {value: 'body', name: 'Содержимое'},
        {value: 'date', name: 'Дата'}
      ]
    }
  },

  methods: {
    createPost(post) {
      this.editedPost = ''
      this.dialogVisible = false;
      let groupFound = ''
      try {
        for (let i = 0; i < this.dataGroup.length; i++) {
          if (post.title === this.dataGroup[i][0].title) {
            this.dataGroup[i].push(post)
            groupFound = true
          }
        }
      }
      finally {
        let newArray = []
        if (groupFound !== true) {
          newArray.push(post)
          this.dataGroup.push(newArray)
        }
      }
    },
    removePost(post) {
      for (let i = 0; i < this.dataGroup.length; i++) {
        this.dataGroup[i] = this.dataGroup[i].filter(p => p.id !== post.id)
      }
    },
    updatePost(post) {
      for (let i = 0; i < this.dataGroup.length; i++) {
        this.dataGroup[i] = this.dataGroup[i].filter(p => p.id !== post.id)
      }
      this.editedPost = post
      this.showDialog()
    },
    showDialog() {
      this.dialogVisible = true;
    },

    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      }
      catch (e) {
        alert('ошибка')
      }
      finally {
        this.isPostsLoading = false;
        let Data = []
        for (let i = 0; i < this.posts.length; i++) {
          Data.push(this.posts[i].title)
        }
        let uniqueData = [...new Set(Data)]
        this.dataGroup = [];
        for (let i = 0; i < uniqueData.length; i++) {
          let temp = []
          for (let j = 0; j < this.posts.length; j++) {
            if (this.posts[j].title === uniqueData[i]) {
              temp.push(this.posts[j])
            }
          }
          this.dataGroup.push(temp)
        }
      }
    },


    async loadMorePosts() {
      try {
        this.page +=1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
        this.dataGroup = []
        let Data = []
        for (let i = 0; i < this.posts.length; i++) {
          Data.push(this.posts[i].title)
        }
        let uniqueData = [...new Set(Data)]
        this.dataGroup = [];
        for (let i = 0; i < uniqueData.length; i++) {
          let temp = []
          for (let j = 0; j < this.posts.length; j++) {
            if (this.posts[j].title === uniqueData[i]) {
              temp.push(this.posts[j])
            }
          }
          this.dataGroup.push(temp)
        }
      }
      catch (e) {
        alert('ошибка')
      }
    },
  },

  mounted() {
    this.fetchPosts();
    /*const options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    const callback = (entries, observer) => {
      if(entries[0].isIntersecting && this.page.length < this.totalPages) {
        this.loadMorePosts()

      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer)*/
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      })
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
    /*groupingPosts() {
      let Data = []
      for (let i = 0; i < this.posts.length; i++) {
        Data.push(this.posts[i].title)
      }
      let uniqueData = [...new Set(Data)]

      let dataGroup = [];
      for (let i = 0; i < uniqueData.length; i++) {
        let temp = []
        for (let j = 0; j < this.posts.length; j++) {
          if (this.posts[j].title === uniqueData[i]) {
            temp.push(this.posts[j])
          }
        }
        dataGroup.push(temp)
      }
      console.log(dataGroup)
      console.log(this.posts)
      return dataGroup[0]

    },*/
  },
  watch: {
    /*page() {
      this.fetchPosts()
    }*/
  }
}
</script>

<style>
.page__wrapper {
  display:flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid green;
}

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.observer {
  height: 30px;
  background: white;
}

</style>