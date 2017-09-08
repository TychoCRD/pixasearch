<template>
  <div class="searchbox-container" :style="searchBoxStyle">
    <div class="title" :class="{'results-title': !isLandingMode}">Pixasearch</div>
    <section class="search-controls">
      <div class="main-input">
        <input type="text" v-model="searchTerm" :placeholder="placeholder"/>
        <button @click.prevent="startSearch">Search</button>
      </div>
      <div class="search-filters">
        
      </div>
    </section>
  </div>
</template>
<script>
export default {
  name: 'SearchBox',
  props: {
    isLandingMode: Boolean
  },
  data () {
    return {
      searchTerm: '',
      placeholder: 'Whatcha looking for?'
    }
  },
  computed: {
    searchBoxStyle () {
      if (this.isLandingMode) {
        return {
          'height': '100%',
          'flex-direction': 'column'
        }
      } else {
        return {
          'height': '65px',
          'flex-direction': 'row'
        }
      }
    }
  },
  methods: {
    startSearch () {
      if (!this.searchTerm) return
      this.$emit('startSearch')
    }
  }
}
</script>

<style lang="scss" scoped>
  .searchbox-container {
    display: flex;
    justify-content: center;
    align-items: center;
    .title {
      font-weight: bold;
      font-size: 34px;
    }
    .results-title {
      margin-right: 30px;
    }
    .search-controls {
      display: flex;
      flex-direction: column;
      .main-input {
        input {
          width: 250px;
        }
      }
    }
  }
</style>
