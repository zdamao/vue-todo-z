<template>
  <div class="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label for>Employee name</label>
      <input
        type="text"
        v-model="employee.name"
        v-bind:class="{'has-error': submitting && invalidName}"
        v-on:focus="clearStatus"
        ref="first"
      />
      <label for>Employee email</label>
      <input
        type="text"
        v-model="employee.email"
        v-bind:class="{'has-error': submitting && invalidEmail}"
        v-on:focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      employee: {
        name: "",
        email: "",
      },
      submitting: false,
      error: false,
      success: false,
    };
  },
  computed: {
    invalidName: function () {
      return this.employee.name === "";
    },
    invalidEmail: function () {
      return this.employee.email === "";
    },
  },
  methods: {
    handleSubmit: function () {
      this.submitting = true;
      this.clearStatus();
      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }
      this.$emit("add:employee", this.employee);
      //this.$refs.first.focus()需要配置在变量设置前面，否则变量不会改变
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: "",
      };
      this.submitting = false;
      this.error = true;
      this.success = true;
      // console.log(this.success);
      // console.log(this.error);
    },
    clearStatus: function () {
      this.error = false;
      this.success = false;
    },
  },
};
</script>

<style lang='scss' scoped>
</style>
