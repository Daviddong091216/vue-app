<template>
  <div id="employee-form">
    <!-- when submit call the handleSubmit methods -->
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <!-- After submitting the form, it would be nice if the focus went back on the first item to make it easy to add many items without clicking around.  -->
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Employee Email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields
      </p>
      <p v-if="success" class="success-message">
        ✅ Employee successfully added
      </p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }
      // Emit broadcasts a name of an event and data to its parent component.
      this.$emit("add:employee", this.employee);
      this.$refs.first.focus()
      this.employee = {
        name: "",
        email: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },

    invalidEmail() {
      if (this.employee.email === "" || this.employee.email.includes("@")) {
        return false;
      } else {
        return true;
      }
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
