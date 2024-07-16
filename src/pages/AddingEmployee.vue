<template>
    <div class="adding-employee" @click.stop>
        <div class="adding-employee_header">
            <div class="title">Добавление пользователя</div>
            <div class="actions">
                <IconClose
                    :size="16"
                    @click="closeModal"
                />
            </div>
        </div>
        <div
             class="adding-employee_body"
        >
            <div v-for="(property, keyProperty) in employee"
                 class="line"
                 :key="keyProperty"
            >
                <label :for="keyProperty">{{property.label}}</label>
                <component
                    class="component_width"
                    :is="property.component"
                    :model="property.model"
                    v-bind="property.props"
                    @inputData="property.setData"
                />
            </div>
        </div>
        <div class="adding-employee_footer">
            <CustomButton
                text="Сохранить"
                :disabled="disableSaveButton"
                @click="saveEmployeeData"/>
        </div>
    </div>
</template>
<script>
import IconClose from '../assets/IconClose.vue'
import CustomInput from '../components/CustomInput.vue'
import DropDown from '../components/DropDown.vue'
import CustomButton from '../components/CustomButton.vue'

export default {
  name: 'AddingEmployee',
  components: {
    CustomButton,
    IconClose,
    CustomInput,
    DropDown
  },
  data () {
    return {
      employee: {
        name: {
          label: 'Имя',
          component: 'CustomInput',
          model: '',
          props: {
            name: 'name'
          },
          setData: (data) => {
            this.employee.name.model = data
          }
        },
        phone: {
          label: 'Телефон',
          model: '',
          component: 'CustomInput',
          props: {
            name: 'phone'
          },
          setData: (data) => {
            this.employee.phone.model = data
          }
        },
        boss: {
          label: 'Начальник',
          component: 'DropDown',
          model: '',
          props: {
            name: 'boss',
            dataList: []
          },
          setData: (data) => {
            this.employee.boss.model = data
          }
        }
      },
      allEmployees: []
    }
  },
  created () {
    this.allEmployees = JSON.parse(localStorage.getItem('employees')) || []
    if (this.allEmployees.length) {
      this.employee.boss.props.dataList = this.allEmployees.map(employee => {
        return {
          name: employee.name,
          id: employee.id
        }
      })
    }
  },
  computed: {
    disableSaveButton () {
      if (!this.employee.name.model || !this.employee.phone.model) return true
      return false
    }
  },
  methods: {
    closeModal () {
      this.$root.$emit('closeModal')
    },
    saveEmployeeData () {
      const employeeData = {}
      for (const key in this.employee) {
        employeeData[key] = this.employee[key].model
      }
      employeeData.id = this.allEmployees.length
      this.allEmployees.push(employeeData)
      localStorage.setItem('employees', JSON.stringify(this.allEmployees))
      this.$root.$emit('employeeAdded', this.allEmployees)
      this.closeModal()
    }
  }
}
</script>
<style scoped>
.adding-employee {
    width: 22rem;
    padding: 1rem;
    background-color: #ffffff;
    border-radius: 6px;
}
.adding-employee_header {
    display: flex;
    justify-content: space-between;
    font-weight: 600;
}
.line {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;
}
.component_width {
    max-width: 50%;
}
.adding-employee_footer {
    margin-top: 1rem;
}
</style>
