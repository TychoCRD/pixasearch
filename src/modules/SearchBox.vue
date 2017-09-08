<template>
  <div class="searchbox-container">
    <div class="search-form">
      <input type="text" v-model="keyword" :placeholder="placeholder"/>
      <button @click.prevent="startSearch"></button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'SearchBox',
  data () {
    return {
      keyword: '',
      placeholder: 'Tell me what image you want!'
    }
  },
  methods: {
    startSearch () {
      if (!this.keyword) return
      alert(keyword)
    }
  }
}
</script>

<style lang="scss" scoped>
  .searchbox-container {
    display: flex;
    justify-content: center;
  }
</style>
