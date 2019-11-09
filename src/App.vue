<template>
  <div id="app" class="small-container">
    <h1>Employee Database <span>✨</span></h1>
    <nasa-picture
      :picture="picture"
    />
    <employee-form
      @add:employee="addEmployee"
    />
    <employee-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'
  import NasaPicture from '@/components/NasaPicture.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm,
      NasaPicture
    },
    data() {
      return {
        employees: [],
        picture: {},
      }
    },

    mounted() {
      this.getEmployees(),
      this.getNasaPicture()
    },

    methods: {
      async getEmployees() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch (error) { // eslint-disable-next-line no-console
          console.error(error);
        }
      },
      async getNasaPicture() {
        var apiKey = 'XUa1X6jQD7DSsdaLShpxfuqkV1tOhbJgj1S4Ud6c';
        try {
          const response = await fetch('https://api.nasa.gov/planetary/apod?api_key=' + apiKey)
          const nasaData = await response.json()
          this.picture = nasaData
        } catch (error) { // eslint-disable-next-line no-console
          console.error(error);
        }
      },
      async addEmployee(employee) {
        // code to generate an id since this is not connected to a DB
        // const lastId =
        //   this.employees.length > 0
        //     ? this.employees[this.employees.length - 1].id
        //     : 0;
        // const id = lastId + 1;
        // const newEmployee = { ...employee, id };

        // this.employees = [...this.employees, newEmployee]

        // Implement asynchronous API calls instead:
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          })
          const data = await response.json()
          this.employees = [...this.employees, data]
        } catch (error) { // eslint-disable-next-line no-console
          console.error(error);
        }
      },
      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'DELETE'
          });
          this.employees = this.employees.filter(
            employee => employee.id !== id
          )
        } catch (error) { // eslint-disable-next-line no-console
          console.error(error);
        }
      },
      async editEmployee(id, updatedEmployee) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = this.employees.map(employee =>
            employee.id === id ? data : employee
          )
        } catch (error) { // eslint-disable-next-line no-console
          console.error(error);
        }
      }
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

  h1 {
    font-weight: bold;
    color: #f5d75f;
    font-size: 45px;
    text-shadow:
    -1px -1px 0 #403e37,
    0   -1px 0 #403e37,
    1px -1px 0 #403e37,
    1px  0   0 #403e37,
    1px  1px 0 #403e37,
    0    1px 0 #403e37,
  -1px  1px 0 #403e37,
  -1px  0   0 #403e37;
    margin-top: 10px;
    margin-top: 10px;
    margin-left: -16px;
  }

  h1 span {
    text-shadow: none;
  }
</style>