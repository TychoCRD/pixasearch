<template>
  <div class="app">
    <search-box></search-box>
  </div>
</template>

<script>
import SearchBox from 'modules/SearchBox'

export default {
  name: 'app',
  components: {
    SearchBox
  }
}
</script>

<style lang="scss">
  .app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
