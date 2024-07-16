<template>
  <div class="table__row_wrapper">
    <div
      class="table__row"
    >
      <div class="table__col name">
        <div v-if="getSubordinate.subordinateOfParent.length"
             @click="showSubordinate = !showSubordinate"
              class="col_action-show"
        >
          +
        </div>
        <div v-else class="col_action-show" />
        <div>
          {{employee.name}}
        </div>
      </div>
      <div class="table__col phone">
        {{employee.phone}}
      </div>
    </div>
    <template v-if="getSubordinate && getSubordinate.subordinateOfParent.length && showSubordinate">
      <RecursiveRow v-for="(subEmployee) in getSubordinate.subordinateOfParent"
            class="subordinate"
            :employee="subEmployee"
           :key="subEmployee.id"
           :allEmployee="getSubordinate.allEmployee"
      />
    </template>
  </div>
</template>
<script>
export default {
  name: 'RecursiveRow',
  components: {
    RecursiveRow: () => import('./RecursiveRow.vue')
  },
  props: {
    employee: {
      type: Object
    },
    allEmployee: {
      type: Object
    },
    sortKey: {
      default: '',
      type: String
    }
  },
  data () {
    return {
      showSubordinate: false
    }
  },
  computed: {
    getSubordinate () {
      const subordinateOfParent = []
      const allEmployee = {}
      for (const key in this.allEmployee) {
        const employeeData = this.allEmployee[key]
        if (parseInt(employeeData.boss) === this.employee.id) {
          subordinateOfParent.push(employeeData)
        } else {
          allEmployee[key] = {}
          allEmployee[key] = this.allEmployee[key]
        }
      }
      if (this.sortKey) {
        subordinateOfParent.sort((a, b) => {
          if (a[this.sortKey] > b[this.sortKey]) return 1
          if (a[this.sortKey] < b[this.sortKey]) return -1
          return 0
        })
      }
      return { subordinateOfParent, allEmployee }
    }
  }
}
</script>
<style scoped>
.subordinate {
  margin-left: 1rem;
}
.subordinate .table__row {
  width: auto;
}
.table__row .name {
  display: flex;
  flex-grow: 4;
  gap: 4px;
}
.phone {
  flex-basis: 20rem;
}
.col_action-show {
  width: 1rem;
  cursor: pointer;
}
</style>
