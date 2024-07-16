<template>
  <div class="main-table_wrapper">
    <div class="button_wrapper">
      <CustomButton
        text="Добавить"
        @click="createEmployee()"
      />
    </div>
    <div v-if="employeeData && employeeData.length" class="table">
      <div class="table__row table_header">
        <div class="table__col" @click="setSortKey('name')">Имя</div>
        <div class="table__col" @click="setSortKey('phone')">Телефон</div>
      </div>
      <RecursiveRow v-for="(employee, indexEmployee) in getEmployee.employeeParent"
                    :employee="employee"
                    :allEmployee="getEmployee.allEmployee"
                    :sortKey="sortKey"
                    :key="indexEmployee"
      />
    </div>
    <div v-else class="empty-data">
      Пока пусто. Добавьте данные о сотрудниках.
    </div>
  </div>
</template>
<script>
import CustomButton from '../components/CustomButton.vue'
import RecursiveRow from '../components/RecursiveRow.vue'

export default {
  name: 'MainTable',
  components: {RecursiveRow, CustomButton},
  data () {
    return {
      employeeData: [],
      sortKey: ''
    }
  },
  mounted () {
    this.employeeData = JSON.parse(localStorage.getItem('employees')) || []
    this.$root.$on('employeeAdded', this.addEmployee)
  },
  computed: {
    getEmployee () {
      let employeeParent = []
      const allEmployee = {}
      this.employeeData.forEach(employee => {
        if (!employee.boss) employeeParent.push(employee)
        else allEmployee[employee.id] = employee
      })
      if (this.sortKey) {
        employeeParent.sort((a, b) => {
          if (a[this.sortKey] > b[this.sortKey]) return 1
          if (a[this.sortKey] < b[this.sortKey]) return -1
          return 0
        })
      }
      return { employeeParent, allEmployee }
    }
  },
  methods: {
    createEmployee () {
      this.$root.$emit('openModal', {name: 'addingEmployee', attrs: {}})
    },
    addEmployee (data) {
      this.employeeData = data
    },
    setSortKey (key) {
      this.sortKey = key
    }
  }
}
</script>

<style scoped>
.main-table_wrapper {
  margin: 6rem auto;
  max-width: 40rem;
}
.button_wrapper {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 2rem;
}
.empty-data {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: auto;
}
.table {
  min-width: 40rem;
}
.table_header {
  background-color: #cbc9c9 !important;
  cursor: pointer;
}
.table_header .table__col {
  width: 20rem;
  margin: 0;
  text-align: center;
}
</style>
