<template>
  <div class="select-container">
    <span>{{ label }}</span>
    <select v-model="selected" class="custom-select">
      <option v-for="option in options" v-bind:value="option.value">
        {{ option.text }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'SelectMenu',
  props: {
    param: String,
    label: String,
    options: Array
  },
  data () {
    return {
      selected: this.options[0].value
    }
  },
  watch: {
    selected (val) {
      this.$emit('change', this.param, val)
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
