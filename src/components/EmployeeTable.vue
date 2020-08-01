<template>
  <div class="employee-table">
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" v-bind:key="employee.id">
          <!-- 遍历employees数据，如果id和点击事件设置的editing相等，就显示input,否则仅显示文本 -->
          <td v-if="employee.id === editing">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="employee.id == editing">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{employee.email}}</td>
          <td v-if="employee.id == editing">
            <button v-on:click="editEmployee(employee)">Save</button>
            <button v-on:click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <button v-on:click="editMode(employee)">Edit</button>
            <button v-on:click="deleteEmployee(employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array,
  },
  data() {
    return {
      editing: null,
    };
  },
  methods: {
    deleteEmployee: function (id) {
      this.$emit("delete:employee", id);
    },
    editMode: function (employee) {
      // Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
      this.cachedEmployee = Object.assign({}, employee);
      //通过绑定在edit按钮上的点击事件获取被点击的employee ID
      this.editing = employee.id;
    },
    editEmployee: function (employee) {
      //如果用户清空数据，则直接返回（不能保存）
      console.log(employee.id);
      if (employee.name === "" || employee.email === "") return;
      // this.$emit("edit:employee", employee.id, employee);
      this.$emit("edit:employee", employee.id, employee);
      // //将editing模式设置为空以结束编辑状态
      this.editing = null;
    },
    cancelEdit: function (employee) {
      Object.assign(employee, this.cachedEmployee);
      this.editing = null;
    },
  },
};
</script>

<style lang='scss' scoped>
</style>
