<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form v-on:add:employee="onAddEmployee" />
    <employee-table
      v-bind:employees="employees"
      v-on:delete:employee="onDeleteEmployee"
      v-on:edit:employee="onEditEmployee"
    />
  </div>
</template>

<script>
import EmployeeForm from "@/components/EmployeeForm.vue";
import EmployeeTable from "@/components/EmployeeTable.vue";

export default {
  name: "App",
  components: {
    EmployeeForm,
    EmployeeTable,
  },
  data() {
    return {
      employees: [],
    };
  },
  methods: {
    getEmployees: async function () {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.log(error);
      }
    },
    onAddEmployee: async function (employee) {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
          {
            method: "POST",
            body: JSON.stringify(employee),
            headers: { "Content-Type": "application/json; charset=UTF-8" },
          }
        );
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.log(error);
      }
    },
    onDeleteEmployee: async function (id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE",
        });
        this.employees = this.employees.filter(
          (employee) => employee.id !== id
        );
      } catch (error) {
        console.log(error);
      }
      // this.employees = this.employees.filter((employee) => employee.id !== id);
    },
    onEditEmployee: async function (id, updatedEmployee) {
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users/${id}`,
          {
            method: "PUT",
            body: JSON.stringify(updatedEmployee),
            headers: { "Content-Type": "application/json; charset=UTF-8" },
          }
        );
        const data = await response.json();
        this.employees = this.employees.map((employee) => {
          return employee.id === id ? data : employee;
        });
      } catch (error) {
        console.log(error);
      }
      // // 箭头函数的大括号需要显示return才能返回值，如果单行则不需大括号也会自动返回值
      // this.employees = this.employees.map((employee) => {
      //   return employee.id === id ? updatedEmployee : employee;
      // });
      // // this.employees = this.employees.map((employee) =>
      // //   employee.id === id ? updatedEmployee : employee
      // // );
    },
  },
  mounted() {
    this.getEmployees();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
form {
  text-align: left;
}
button {
  margin: 0 1rem 0 0;
}
</style>
