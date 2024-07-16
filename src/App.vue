<template>
  <div id="app">
    <MainTable
      :class="{blur: modalComponentData}"
    />
    <div v-if="modalComponentData" class="modal" @click="closeModal">
      <component
        :is="modalComponentData.name"
        v-bind="modalComponentData.attrs"
      />
    </div>
  </div>
</template>

<script>

import MainTable from './pages/MainTable.vue'

export default {
  name: 'App',
  components: {
    MainTable,
    addingEmployee: () => import('./pages/AddingEmployee.vue')
  },
  data () {
    return {
      modalComponentData: false
    }
  },
  mounted () {
    this.$root.$on('openModal', this.setModalComponent)
    this.$root.$on('closeModal', this.closeModal)
  },
  methods: {
    setModalComponent (componentData) {
      this.modalComponentData = componentData
    },
    closeModal () {
      this.modalComponentData = false
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  display: flex;
  justify-content: center;
}
.modal {
  position: fixed;
  top: 0;
  left: 0 ;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(54, 56, 56, 0.1);
}
.blur {
  filter: blur(4px);
}
.table {
  background-color: #f8f6f6;
}
.table__row {
  width: 40rem;
  display: flex;
  justify-content: flex-end;
  background-color: #e5e1e1;
}
.table__col {
  margin: 0 0.5rem;
}
</style>
