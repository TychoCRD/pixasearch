<template>
  <div class="app">
    <search-box 
      @startSearch="handleNewSearch"
      :isLandingMode="isLandingMode"></search-box>
    <results v-if="!isLandingMode" :isLoading="isLoading" 
      @openImage="openModal"
      @loadNewPage="loadNewPage"
      :page="searchRequest.page"
      :list="resultsList" 
      :totalResults="totalResults"></results>
    <image-modal @close="closeModal"
      :isVisible="showModal"
      :startIndex="modalStartIndex"
      :list="resultsList"></image-modal>
  </div>
</template>

<script>
import SearchBox from './modules/SearchBox/index.vue'
import Results from './modules/Results.vue'
import ImageModal from './modules/ImageModal.vue'
import config from '../config.js'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    SearchBox,
    Results,
    ImageModal
  },
  data () {
    return {
      isLandingMode: true,
      searchRequest: null,
      modalStartIndex: null,
      isLoading: false,
      totalResults: 0,
      resultsList: []
    }
  },
  computed: {
    showModal () {
      return !!this.modalStartIndex
    }
  },
  methods: {
    handleNewSearch (request) {
      if (this.isLandingMode) {
        this.isLandingMode = false
      }
      this.searchRequest = request
      this.makeSearchRequest()
    },
    loadNewPage (direction) {
      if (direction === 'prev') {
        this.searchRequest.page--
      } else {
        this.searchRequest.page++
      }
      this.makeSearchRequest()
    },
    makeSearchRequest () {
      let params = Object.assign({}, this.searchRequest)
      params.key = config.key
      this.isLoading = true
      axios.get('https://pixabay.com/api/', { params })
        .then(resp => {
          console.log(resp)
          if (resp.status === 200) {
            this.totalResults = resp.data.totalHits
            this.resultsList = resp.data.hits
          }
          this.isLoading = false
        })
    },
    openModal (index) {
      this.modalStartIndex = index
    },
    closeModal () {
      this.modalStartIndex = null
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
    display: flex;
    flex-direction: column;
  }
</style>
