<template>
  <div class="app">
    <search-box 
      @startSearch="handleSearchRequest"
      :isLandingMode="isLandingMode"></search-box>
  </div>
</template>

<script>
import SearchBox from './modules/SearchBox/index.vue'
import config from '../config.js'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    SearchBox
  },
  data () {
    return {
      isLandingMode: true,
      searchRequest: null,
      totalResults: 0,
      resultsList: []
    }
  },
  methods: {
    handleSearchRequest (request) {
      if (this.isLandingMode) {
        this.isLandingMode = false
      }
      this.searchRequest = request
      let params = Object.assign({}, this.searchRequest)
      params.key = config.key
      axios.get('https://pixabay.com/api/', { params })
        .then(resp => {
          console.log(resp)
          if (resp.status === 200) {
            this.totalResults = resp.data.totalHits
            this.resultsList = resp.data.hits
          }
        })
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
