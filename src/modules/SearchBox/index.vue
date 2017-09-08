<template>
  <div class="searchbox" :class="isLandingMode ? '' : 'searchbox-results'">
    <div class="title">Pixasearch</div>
    <section class="search-controls">
      <div class="main-input">
        <input type="text" class="form-control" 
          v-model="searchTerm" 
          :placeholder="placeholder" autofocus/>
        <button class="btn btn-primary"
          @click.prevent="startSearch">Search</button>
      </div>
      <div class="search-filters">
        <select-menu v-for="filter in filtersList"
          :key="filter.param"
          @change="updateSearchParams"
          :param="filter.param"
          :label="filter.label"
          :options="filter.options"></select-menu>
      </div>
    </section>
  </div>
</template>
<script>
import SelectMenu from '../../components/SelectMenu.vue'
import filters from './filters.js'

export default {
  name: 'SearchBox',
  props: {
    isLandingMode: Boolean
  },
  components: {
    SelectMenu
  },
  data () {
    return {
      searchTerm: '',
      placeholder: 'Whatcha looking for?',
      searchParams: {
        image_type: 'all',
        orientation: 'all',
        category: '',
        min_width: 0,
        min_height: 0,
        editors_choice: false,
        safesearch: false,
        order: 'popular'
      }
    }
  },
  computed: {
    filtersList () {
      const filterMap = [
        {
          param: 'image_type',
          label: 'Type:',
          options: filters['image_type']
        },
        {
          param: 'orientation',
          label: 'Orientation:',
          options: filters['orientation']
        },
        {
          param: 'category',
          label: 'Category:',
          options: filters['category']
        },
        {
          param: 'order',
          label: 'Order By:',
          options: filters['order']
        }
      ]
      return filterMap
    }
  },
  methods: {
    startSearch () {
      if (!this.searchTerm) return
      const defaultParams = {
        q: '',
        page: 1,
        per_page: 200
      }
      let request = Object.assign({}, defaultParams, this.searchParams)
      request.q = this.searchTerm
      this.$emit('startSearch', request)
    },
    updateSearchParams (param, val) {
      this.searchParams[param] = val
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
    padding: 20px;
    .title {
      font-weight: bold;
      font-size: 60px;
    }
    .search-controls {
      display: flex;
      flex-direction: column;
      .main-input {
        display: flex;
        input {
          width: 250px;
          margin-right: 10px;
        }
        margin-bottom: 15px;
      }
      .search-filters {
        div {
          margin-left: 15px;
        }
      }
    }
  }
  .searchbox-results {
    flex-direction: row;    
    flex-wrap: wrap;
    justify-content: center;
    height: 65px;
    margin-top: 0;
    .title {
      margin-right: 30px;
      font-size: 34px;
    }
    .search-controls {
      flex-direction: row;
      flex-wrap: wrap;
    }
  }
</style>
