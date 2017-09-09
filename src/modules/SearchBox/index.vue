<template>
  <div class="searchbox" :class="isLandingMode ? '' : 'searchbox-results'">
    <section class="title-search">
      <h2 class="title">Pixasearch</h2>
      <form class="main-input" @submit.prevent="startSearch">
        <input type="text" class="form-control" 
          v-model="searchTerm" 
          :placeholder="placeholder" autofocus/>
        <button class="btn btn-primary">Search
          <i class="fa fa-search"></i>
          </button>
      </form>
    </section>
    <section class="search-filters">
      <select-menu v-for="filter in filtersList"
        :key="filter.param"
        @change="updateSearchParams"
        :param="filter.param"
        :label="filter.label"
        :options="filter.options"></select-menu>
        <div class="form-group">
          <span>Min Width:</span>
          <input class="form-control" type="number" id="min_width" v-model="searchParams.min_width"/>
        </div>
        <div class="form-group">
          <span>Min Height:</span>
          <input class="form-control" type="number" id="min_height" v-model="searchParams.min_height"/>
        </div>
        <div class="form-check">
          <label for="editor" class="form-check-label">
            <input class="form-check-input" type="checkbox" id="editor" v-model="searchParams.editors_choice">
            Editor's Choice</label>
        </div>
        <div class="form-check">
          <label for="safe" class="form-check-label">
            <input class="form-check-input" type="checkbox" id="safe" v-model="searchParams.safesearch">
            Safesearch</label>
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
    flex: 1;
    display: flex;
    align-items: center;
    flex-direction: column;
    // height: 100%;
    padding: 20px;
    .title-search {
      display: flex;
      flex-direction: column;
      .title {
        font-weight: bold;
        // font-size: 44px;
      }
      .main-input {
        display: flex;
        input {
          // font-size: 26px;
          // width: 250px;  
          // width: 300px;
          margin-right: 10px;
        }
        margin-bottom: 15px;
      }
    }
    .search-filters {
      display: flex;
      flex-wrap: wrap;
      width: 65%;
      div {
        display: flex;
        align-items: center;
        margin-left: 15px;
        margin-bottom: 15px;
      }
      input[type="number"] {
        width: 70px;
      }
      .form-group {
        span {
          margin-right: 10px;
        }
      }
      .form-check {
        margin-left: 25px;
      }
    }
  }
  .searchbox-results {
    flex-shrink: 0;
    flex-direction: row;    
    flex-wrap: wrap;
    justify-content: center;
    height: 130px;
    border-bottom: 1px solid #ccc;
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
