<template>
  <select
    class="drop-down"
    :name="name"
    @input="selectData">
    <option value="">Выберите начальника</option>
    <option v-for="(item, indexItem) in dataList"
            :value="item.id"
            :key="indexItem"
    >{{item.name}}</option>
  </select>
</template>
<script>
export default {
  name: 'DropDown',
  props: {
    name: {
      type: String,
      default: ''
    },
    dataList: {
      type: Array
    },
    model: {
      type: String
    }
  },
  methods: {
    selectData (event) {
      this.$emit('inputData', event.target.value)
    }
  }
}
</script>
<style scoped>
.drop-down {
  width: 100%;
}
</style>
