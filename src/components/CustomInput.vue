<template>
  <input class="input"
    :name="name"
    :value="model"
    @input="inputModel"/>
</template>
<script>
export default {
  name: 'CustomInput',
  props: {
    model: {
      type: String,
      default: ''
    },
    name: {
      type: String,
      default: ''
    }
  },
  methods: {
    inputModel (event) {
      this.$emit('inputData', event.target.value)
    }
  }
}
</script>
<style scoped>
.input {
  width: 100%;
}
</style>
