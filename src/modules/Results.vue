<template>
  <div class="results-container" v-if="!isLoading && totalResults > 0">
    <thumbnail v-for="(image, index) in list" 
      @openImage="$emit('openImage', index)"
      :index="index"
      :key="image.id"
      :imageData="image"></thumbnail>
  </div>
  <div class="loading" v-else-if="isLoading">
    <h1>Loading...</h1>
  </div>
  <div class="no-results" v-else-if="!isLoading && totalResults === 0">
    <h1>Sorry, no results. Please try again.</h1>
  </div>
</template>

<script>
import Thumbnail from './Thumbnail.vue'

export default {
  name: 'Results',
  props: {
    isLoading: Boolean,
    totalResults: Number,
    list: Array
  },
  components: {
    Thumbnail
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
    justify-content: space-between;
    padding: 20px 30px;
  }
  .loading,
  .no-results {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center
  }

</style>
