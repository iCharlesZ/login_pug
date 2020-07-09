<template>
  <div id="app">
    <Login />
  </div>
</template>

<script>
import Login from "./components/Login.vue";

export default {
  name: "App",
  components: {
    Login
  }
};
</script>

<style lang="sass">
*
  margin: 0
  padding: 0

body
  background: #aeebff
  display: flex
  align-items: center
  justify-content: center
  
  #app
    font-family: Avenir, Helvetica, Arial, sans-serif
    -webkit-font-smoothing: antialiased
    -moz-osx-font-smoothing: grayscale
    margin: 0
    padding: 0
</style>
