<template>
<div class="navbar">
<h2 @click="$router.push('/')" style="cursor: pointer">Vue 3</h2>
  <div class="navbar__btns">
    <my-button @click="$router.push('/posts')">Посты</my-button>
    <my-button style="margin-left: 15px" @click="$router.push('/about')">О сайте</my-button>
    <my-button style="margin-left: 15px" @click="$router.push('/store')">store</my-button>
    <my-button style="margin-left: 15px" @click="$router.push('/composition')">Composition</my-button>
  </div>
</div>
</template>

<script>
export default {
  name: "Navbar"
}
</script>

<style scoped>
.navbar {
  height: 50px;
  background-color: lightblue;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}

.navbar__btns {
  margin-left: auto;
  margin-bottom: 15px;
  cursor: pointer;
}

</style>