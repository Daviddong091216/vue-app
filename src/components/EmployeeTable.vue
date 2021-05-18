<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <!-- for loop -->
        <!--  Vue has a requirement for uniquely identifying any element in an array, so we'll use :key on the table row and set it to a unique value.-->

        <tr v-for="employee in employees" :key="employee.id">
          <!-- To make it editable, we'll check if editing === employee.id is true for a particular row, and display and input instead.  -->
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">
              Cancel
            </button>
          </td>
          <td v-else>
            <button @click="editMode(employee.id)">Edit</button>
            <!-- emit an event to the parent component -->
            <button @click="$emit('delete:employee', employee.id)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  // we tell the component that it will receive props, in this case an Array.
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
      //   cachedEmployee: {}
    };
  },
  methods: {
    editMode(id) {
      this.editing = id;
    },
    // editMode(employee) {
    //   this.cachedEmployee = Object.assign({}, employee);
    //   this.editing = employee.id;
    // },
    // cancelEdit(employee) {
    //   Object.assign(employee, this.cachedEmployee);
    //   this.editing = null;
    // },

    editEmployee(employee) {
      if (employee.name === "" || employee.email === "") return;
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
input {
  margin: 0;
}
.empty-table {
  text-align: center;
}
</style>
