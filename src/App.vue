<template>
  <post-form
      @createPost = createPost />
  <post-list
      :posts="posts"
      @remove="removePost"
       />
</template>

<script>
import PostList from "./components/PostList";
import PostForm from "./components/PostForm";
export default {
  components: {
    PostForm,
    PostList
  },
  name: "App",
  data() {
    return{
        posts: [
          {id: 1, title: 'JavaScript', body: 'Описание поста'},
          {id: 2, title: 'JavaScript 2', body: 'Описание поста 2'},
          {id: 3, title: 'JavaScript 3', body: 'Описание поста 3'},
        ],
    }
  },
  methods: {
      createPost(post) {
        this.posts.push(post)
      },
      removePost(post) {
        this.posts = this.posts.filter(p => p.id !== post.id)
      }
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