<template>
  <div>

  <h1>Страница с постами</h1>
  <my-input
      :modal-value="searchQuery"
      @update:model-value="setSearchQuery"
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
        :model-value="selectedSort"
        @update:model-value="setSelectedSort"
        :options="sortOptions"
    />


  </div>
  <my-dialog v-model:show="dialogVisible">
    <post-form
        @createPost = createPost />
  </my-dialog>

  <post-list
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
  />
  <div v-else>Идёт загрузка...</div>
  <div v-intersection="loadMorePosts" class="observer"></div>
  Пагинация первый способ
    <div class="page__wrapper">
      <div v-for="pageNumber in totalPages"
            :key="pageNumber"
            class="page"
            :class="{'current_page': page === pageNumber
            }"
           @click="changePage(pageNumber)"
            >

        {{ pageNumber }}
      </div>
    </div>
</div>

</template>



<script>
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex'
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
    }
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
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
  },

  mounted() {
    this.fetchPosts()
  },

  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPostsLoading: state => state.post.isPostsLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions

    }),
    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'

    })
  }


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