<template>
  <h1>Страница с постами</h1>
  <div class="app__btns">
    <my-button
        @click="showDialog"

    >
      Создать пользователя
    </my-button>
    <my-select
        v-model="selectedSort"
        :options="sortOptions"
        >


    </my-select>


  </div>
  <my-dialog v-model:show="dialogVisible">
    <post-form
        @createPost = createPost />
  </my-dialog>

  <post-list
      :posts="sortedPosts"
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
      selectedSort: '',
      sortOptions: [
          {value: 'title', name: 'По названию'},
          {value: 'body', name: 'По содержимому'},
      ]
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
  },

  computed: {
      sortedPosts() {
          return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
      },
  },

  //Сортировка решение 2
  // watch: {
  //   selectedSort(newValue) {
  //     this.posts.sort((post1, post2) => {
  //         return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
  //     })
  //   },
  // }

}
</script>

<style>
 * {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
 }

.app__btns {
  margin:15px 0;
  display:flex;
  justify-content: space-between;
}


form {
  display: flex;
  flex-direction: column;
}

</style>