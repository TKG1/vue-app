<template>
  <div>
    <h1>Employees</h1>
    
    <employee-form @add:employee="addEmployee" />
    <employee-table 
      v-bind:employees="employees" 
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
      />
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm,
    },
    data() {
      return {
        employees: [
        ],
      }
    },

    mounted() {
      this.getEmployees()
    },

    methods: {
      addEmployee(employee) {
        fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        .then(response => response.json())
        .then(data => { 
          console.log(data)
          return this.employees = [...this.employees, data]
        })
        .catch(error => {
          console.error(error)
        })
        
      },

      async deleteEmployee(id) {
        try {
          await fetch('https://jsonplaceholder.typicode.com/users/${id}',{
            method: 'DELETE',
          })

          this.employees = this.employees.filter(employee => employee.id !== id)
        } catch(error) {
          console.error(error)
        }
      },

      async editEmployee(id, updateEmployee) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users/${id}',{
            method: 'PUT',
            body: JSON.stringify(updateEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8'}
          })
          const data = await response.json()
          this.employees = this.employees.map(employee =>
            employee.id === id ? data : employee
          )
        } catch(error) {
          console.error(error)
        }
      },

      getEmployees() {
        fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => { 
          console.log(data)
          return this.employees = data 
        })
        .catch(error => {
          console.error(error)
        })
      },
    }
  }
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
