<template>
  <div class="searchbox" :class="isLandingMode ? '' : 'searchbox-results'">
    <div class="title">Pixasearch</div>
    <section class="search-controls">
      <div class="main-input">
        <input type="text" v-model="searchTerm" :placeholder="placeholder" autofocus/>
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
  methods: {
    startSearch () {
      if (!this.searchTerm) return
      this.$emit('startSearch')
    }
  }
}
</script>

<style lang="scss" scoped>
  .searchbox {
    display: flex;
    align-items: center;
    flex-direction: column;
    height: 100%;
    margin-top: 10%;
    .title {
      font-weight: bold;
      font-size: 60px;
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
  .searchbox-results {
    flex-direction: row;    
    justify-content: center;
    height: 65px;
    margin-top: 0;
    .title {
      margin-right: 30px;
      font-size: 34px;
    }
  }
</style>
