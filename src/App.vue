<template>
  <div id="app">
    <img id="startup_loader" v-if="loading" src="./assets/spinner.gif" alt="loading-spinner">
    <Repos @loaded="hideLoading" @fetching="showLoading"/>
  </div>
</template>

<script>
import Repos from './components/Repos/Repos.vue'

export default {
  name: 'app',
  data(){
    return {
      loading: true
    }
  },
  components: {
    Repos
  },
  methods:{
    hideLoading: function() {
      this.loading = false
    },
    showLoading: function() {
      this.loading = true
    }
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
</style>
