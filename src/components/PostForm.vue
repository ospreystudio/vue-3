<template>
  <div class="app" @submit.prevent>
    <p>Создание поста</p>
    <form>
      <my-input  type="text" placeholder="Название" v-model="post.title" />
      <my-input  type="text" placeholder="Описание" v-model="post.body" />
      <my-button class="btn" @click="createPost">Создать</my-button>
    </form>
  </div>
</template>

<script>


export default {
  name: "PostForm",
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {

      this.post.id = Date.now()
      this.$emit('createPost', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
  },
}
</script>

<style scoped>
.app {
  padding: 20px;
}


</style>