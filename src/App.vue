<template>
  <div id="app" class="small-container">
    <h1>WWT Internal IT Application Services Team Employees</h1>
    <!--  employee-form is broadcasting its emitted event, but we need to capture the event and value in the parent to work with it.-->
    <!--make employee-form acknowledge and handle the emitted event, and invoke a new method.  -->
    <!-- Get data from employee-form -->
    <employee-form @add:employee="addEmployee" />
    <!-- match EmployeeTable.vue export name: "employee-table"-->
    <!-- v-bind:employees="employees" pass employees arrary data from App to EmployeeTable-->
    <!-- capture the emit data, get data from its child component -->
    <employee-table
      v-bind:employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "@/components/EmployeeTable.vue";
import EmployeeForm from "./components/EmployeeForm.vue";

export default {
  name: "app",
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: [
        {
          id: 1,
          name: "Sandi St. John",
          email: "sandi@wwt.com"
        },
        {
          id: 2,
          name: "David Dong",
          email: "david@wwt.com"
        },
        {
          id: 3,
          name: "Aaron",
          email: "aaron@wwt.com"
        }
      ]
    };
  },
  methods: {
    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };

      this.employees = [...this.employees, newEmployee];
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(employee => employee.id !== id);
    },
    // It will take id and updatedEmployee parameters, map through the employees array, and update the correct employee.
    editEmployee(id, updatedEmployee) {
      this.employees = this.employees.map(employee =>
        employee.id === id ? updatedEmployee : employee
      );
    }
  }
};
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
