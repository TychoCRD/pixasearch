<template>
  <div class="select-container">
    <span>{{ label }}</span>
    <select @change="updateParam" class="custom-select">
      <option v-for="option in options"
        :selected="option.value === selectedVal"
        :value="option.value">
        {{ option.text }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'SelectMenu',
  props: {
    selectedVal: String,
    param: String,
    label: String,
    options: Array
  },
  methods: {
    updateParam (e) {
      this.$emit('change', this.param, e.target.value)
    }
  }
}
</script>

<style lang="scss" scoped>
  .select-container {
    span {
      margin-right: 10px;
    }
  }
</style>
