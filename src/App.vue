<template>
  <div class="app">
    <search-box 
      @startSearch="handleSearchRequest"
      :isLandingMode="isLandingMode"></search-box>
  </div>
</template>

<script>
import SearchBox from './modules/SearchBox/index.vue'

export default {
  name: 'app',
  components: {
    SearchBox
  },
  data () {
    return {
      isLandingMode: true
    }
  },
  methods: {
    handleSearchRequest () {
      if (this.isLandingMode) {
        this.isLandingMode = false
      }
    }
  }
}
</script>

<style lang="scss">
  html, body {
    height: 100vh;
  }
  .app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100%;
  }
</style>
