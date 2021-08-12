<template>
  <h1>Страница с постами</h1>
  <my-button
      @click="showDialog"
      style="margin:15px 0">

    Создать пользователя
  </my-button>
  <my-dialog v-model:show="dialogVisible">
    <post-form
        @createPost = createPost />
  </my-dialog>

  <post-list
      :posts="posts"
      @remove="removePost"
      v-if="!isPostsLoading"
       />
  <div v-else>Идёт загрузка...</div>
</template>

<script>
import PostList from "./components/PostList";
import PostForm from "./components/PostForm";
import axios from "axios"
export default {
  components: {
    PostForm,
    PostList
  },
  name: "App",
  data() {
    return{
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
    }
  },
  methods: {
      createPost(post) {
        this.posts.push(post)
        this.dialogVisible = false
      },
      removePost(post) {
        this.posts = this.posts.filter(p => p.id !== post.id)
      },
      showDialog() {
        this.dialogVisible = true
      },
      async fetchPosts() {
        this.isPostsLoading =true
          try {
              const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
              this.posts = response.data;
          } catch (e) {
            alert('Ошибка')
          } finally {
            this.isPostsLoading = false
          }
      }
  },

  mounted() {
    this.fetchPosts()
  }

}
</script>

<style>
 * {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
 }



 .input {
   width: 100%;
   border: 1px solid teal;
   padding: 10px 15px;
   margin-top: 5px;
 }
form {
  display: flex;
  flex-direction: column;
}

</style>