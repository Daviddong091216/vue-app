<template>
  <div id="employee-table">
    <table>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
        </tr>
      </thead>
      <tbody>
          <!-- for loop -->
          <!--  Vue has a requirement for uniquely identifying any element in an array, so we'll use :key on the table row and set it to a unique value.-->
        <tr v-for="employee in employees" :key="employee.id">
          <td>{{ employee.name }}</td>
          <td>{{ employee.email }}</td>
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
  }
};
</script>

<style scoped></style>
