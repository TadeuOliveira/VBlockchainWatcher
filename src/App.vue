<template>
  <div id="app">
    <b-container>
      <h1>V-Blockchain Watcher</h1>
      <router-view></router-view>
    </b-container>
  </div>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'

  export default {
    name: 'app',
    components: {
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
router-link {
  margin: 20px;
}
</style>
