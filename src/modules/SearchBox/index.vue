<template>
  <div class="searchbox" :class="isLandingMode ? '' : 'searchbox-results'" :style="searchboxHeight">
    <section class="title-search">
      <div class="title-box">
        <span class="title">Pixasearch</span>
        <span class="filter-toggle" v-if="!isLandingMode"
        @click="toggleFilters">Filters <i class="fa" :class="showFilters ? 'fa-angle-up' : 'fa-angle-down'"></i></span>
      </div>
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
        <!-- <span class="reset" @click="resetFilters">Reset <i class="fa fa-refresh"></i></span> -->
    </section>
  </div>
</template>
<script>
import SelectMenu from '../../components/SelectMenu.vue'
import filters from './filters.js'

const defaultSearch = {
  image_type: 'all',
  orientation: 'all',
  category: '',
  min_width: 0,
  min_height: 0,
  editors_choice: false,
  safesearch: false,
  order: 'popular'
}
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
      },
      showFilters: false
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
    },
    searchboxHeight () {
      if (this.isLandingMode) {
        return {
          'height': '100%'
        }
      } else {
        return {
          'height': this.showFilters ? '500px' : '80px'
        }
      }
    }
  },
  methods: {
    startSearch () {
      if (!this.searchTerm) return
      if (this.showFilters) {
        this.showFilters = false
      }
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
    },
    toggleFilters () {
      this.showFilters = !this.showFilters
    }
  }
}
</script>

<style lang="scss" scoped>
  .searchbox {
    // flex: 1;
    position: fixed;
    // height: 100%;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 20px;
    overflow-y: auto;
    z-index: 2;
    background-color: #fff;
    .title-search {
      display: flex;
      flex-direction: column;
      flex-shrink: 0;
      margin-bottom: 15px;
      .title {
        font-size: 34px;
        font-weight: bold;
      }
      .main-input {
        display: flex;
        align-items: center;
        input {
          margin-right: 10px;
        }
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
    // flex-direction: row;    
    // flex-wrap: wrap;
    // justify-content: center;
    // height: 80px;
    justify-content: flex-start;
    border-bottom: 1px solid #ccc;
    box-shadow: 0 0px 10px #ccc;
    overflow: hidden;
    padding: 10px;
    .title-search {
      flex-direction: row;
      align-items: center;
      height: 80px;
      .title-box {
        margin-right: 10px;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        .title {
          font-size: 18px;
        }
        .filter-toggle {
          font-size: 14px;
          cursor: pointer;
          i {
            font-size: 16px;
          }
        }
      }
      .main-input {
        margin-right: 10px;
      }
    }
  }
</style>
