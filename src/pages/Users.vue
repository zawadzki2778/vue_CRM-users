<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <h1>Users Page</h1>
        <table>
          <thead>
            <tr>
              <th>Name</th>
              <th>Age</th>
              <th>Gender</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>
                {{ user.name }}
              </td>
              <td>
                {{ user.age }}
              </td>
              <td>
                {{ user.gender }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "Users",
  data() {
    return {
      users: [
        { id: 1, name: "Maks", age: 44, gender: "M" },
        { id: 2, name: "Vita", age: 42, gender: "G" },
      ],
    };
  },
};
</script>

<style lang="scss">
</style>