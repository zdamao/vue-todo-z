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
      employees: [
        {
          id: 1,
          name: "zdamao",
          email: "zdamao@126.com",
        },
        {
          id: 2,
          name: "adamao",
          email: "adamao@126.com",
        },
        {
          id: 3,
          name: "bdamao",
          email: "bdamao@126.com",
        },
      ],
    };
  },
  methods: {
    onAddEmployee: function (employee) {
      const lastId = this.employees[this.employees.length - 1].id;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };
      this.employees = [...this.employees, newEmployee];
    },
    onDeleteEmployee: function (id) {
      this.employees = this.employees.filter((employee) => employee.id !== id);
    },
    onEditEmployee: function (id, updatedEmployee) {
      console.log(id, updatedEmployee);
      // 箭头函数的大括号需要显示return才能返回值，如果单行则不需大括号也会自动返回值
      this.employees = this.employees.map((employee) => {
        return employee.id === id ? updatedEmployee : employee;
      });
      // this.employees = this.employees.map((employee) =>
      //   employee.id === id ? updatedEmployee : employee
      // );
    },
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
