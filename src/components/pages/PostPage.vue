<template>
  <h1>Страница с постами</h1>
  <my-input
      v-model="searchQuery"
      placeholder="Search..."
      v-focus
  />


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
      :posts="sortedAdnSearchPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
  />
  <div v-else>Идёт загрузка...</div>
  <div v-intersection="loadMorePosts" class="observer"></div>
  <!--Пагинация первый способ    -->
  <!--  <div class="page__wrapper">-->
  <!--    <div v-for="pageNumber in totalPages"-->
  <!--          :key="pageNumber"-->
  <!--          class="page"-->
  <!--          :class="{'current_page': page === pageNumber-->
  <!--          }"-->
  <!--         @click="changePage(pageNumber)"-->
  <!--          >-->

  <!--      {{ pageNumber }}-->
  <!--    </div>-->
  <!--  </div>-->
  <div ></div>

</template>

<script>
import PostList from "../../components/PostList";
import PostForm from "../../components/PostForm";
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
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
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
    // changePage(pageNumber) {
    //   this.page = pageNumber
    // this.fetchPosts()  отвечает за смену траницы
    // },
    async fetchPosts() {
      this.isPostsLoading =true
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
      } catch (e) {
        alert('Ошибка')
      }
    }
  },

  mounted() {
    console.log(this.$refs.observer)
    this.fetchPosts();
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts()
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer)
  },

  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },

    sortedAdnSearchPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLocaleLowerCase()))
    }
  },

  //Сортировка решение 2
  // watch: {
  //   selectedSort(newValue) {
  //     this.posts.sort((post1, post2) => {
  //         return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
  //     })
  //   },
  // }

  // watch: {
  //   page() {
  // this.fetchPosts() // отбражение страниц с помощью наблюдателя
  //   }
  // }

}
</script>

<style>



form {
  display: flex;
  flex-direction: column;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
  margin:5px;
  cursor: pointer;
}

.current_page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: green;

}

</style>