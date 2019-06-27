<template>
  <div class="home">
    
    <img alt="Vue logo" src="../assets/logo.png" class="logo"/>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <TodoList/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import TodoList from "@/components/TodoList"

export default {
  name: "home",
  components: {
    // HelloWorld
    TodoList
  }
};
</script>
<style lang="scss">
.home {
  max-width: 500px;
  margin: 0 auto;
}

  .logo{
  display: block;
  margin: 20px auto;
  height: 75px;
}

</style>

