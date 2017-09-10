<template>
  <div class="results-container" v-if="!showPlaceholder">
    <div class="hits-display">{{ `${totalResults} results, displaying ${list.length}.` }}</div>
    <thumbnail v-for="(image, index) in list" 
      @openImage="$emit('openImage', index)"
      :index="index"
      :key="image.id"
      :imageData="image"></thumbnail>
    <div class="page-nav" v-if="showPageNav">
      <div class="move-back" v-if="page > 1" @click="loadNewPage('prev')">
        <i class="fa fa-angle-left"></i>
      </div>
        <h2>{{ `Page ${page}` }}</h2>
      <div class="move-forward" v-if="page < totalPages" @click="loadNewPage('next')">
        <i class="fa fa-angle-right"></i>
      </div>
    </div>
  </div>
  <div class="placeholder" v-else-if="showPlaceholder">
    <h1 v-if="isLoading">Loading...</h1>
    <h1 v-else>Sorry, no results. Please try again.</h1>
  </div>
</template>

<script>
import Thumbnail from './Thumbnail.vue'

export default {
  name: 'Results',
  props: {
    isLoading: Boolean,
    page: Number,
    totalResults: Number,
    list: Array
  },
  components: {
    Thumbnail
  },
  computed: {
    showPlaceholder () {
      return this.isLoading || (!this.isLoading && this.totalResults === 0)
    },
    totalPages () {
      return Math.ceil(this.totalResults / 200)
    },
    showPageNav () {
      return this.totalPages > 0
    }
  },
  methods: {
    loadNewPage (direction) {
      this.$emit('loadNewPage', direction)
      this.$el.scrollTop = 0
    }
  }
}
</script>

<style lang="scss" scoped>
  .results-container {
    flex: 1;
    overflow-y: auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    align-content: stretch;
    justify-content: flex-start;
    padding: 20px 30px;
    margin-top: 90px;
    .hits-display {
      flex-shrink: 0;
      width: 100%;
      height: 20px;
      margin-bottom: 15px;
    }
    .page-nav {
      flex-shrink: 0;
      width: 100%;
      height: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
      h2 {
        margin: 0 10px;
      }
      .move-back,
      .move-forward {
        color: #007bff;
        font-size: 40px;
        cursor: pointer;
      } 
    }
  }
  .placeholder {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center
  }

</style>
