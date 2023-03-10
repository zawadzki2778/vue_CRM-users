<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th @click="sort('name')">Name &#8597;</th>
              <th @click="sort('age')">Age &#8597;</th>
              <th @click="sort('gender')">Gender &#8597;</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td>
                <img :src="user.img" :alt="user.name" />
                <span>{{ user.name }}</span>
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
        <p style="text-alain: center">
          <span>debug: sort: {{ currentSort }}, dir: {{ currentSortDir }}</span>
          <span>page: {{ page.current }} length: {{ page.length }}</span>
        </p>
      </div>
    </section>

    <section>
      <div class="conteiner">
        <div class="button-list">
          <div class="btn btnPrimary" @click="backPage">&#60;</div>
          <div class="btn btnPrimary" @click="nextPage">&#62;</div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Users",
  data() {
    return {
      users: [],
      currentSort: "name",
      currentSortDir: "asc",
      page: {
        current: 2,
        length: 5,
      },
    };
  },
  created() {
    axios
      .get("https://tocode.ru/static/_secret/courses/1/usersCrmApi.php")
      .then((res) => (this.users = res.data))
      .catch((err) => console.log(err));
  },
  computed: {
    usersSort() {
      return this.users
        .sort((a, b) => {
          let mod = 1;
          if (this.currentSortDir === "desc") mod = -1;
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod;
          return 0;
        })
        .filter((row, index) => {
          let start = (this.page.current - 1) * this.page.length;
          let end = this.page.current * this.page.length;
          // if (index >= start && index < end) return true;
          return index >= start && index < end;
        });
    },
  },
  methods: {
    /* ------- S O R T -------- */
    sort(column) {
      // column это name, age, gender
      if (this.currentSort === column) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      } else {
        this.currentSort = column;
        this.currentSortDir = "asc";
      }
    },
    /* ****** не работала сортировка при повторном клике******* */
    // sort(event) {
    //   if (event === this.currentSort) {
    //     this.currentSortDir = this.currentSort === "asc" ? "desc" : "asc";
    //   }
    //   this.currentSort = event;
    // },

    /* ---- P A G I N A T I O N ---- */
    backPage() {
      if (this.page.current > 1) this.page.current -= 1;
    },
    nextPage() {
      if (this.page.current * this.page.length < this.users.length)
        this.page.current += 1;
    },
  },
};
</script>

<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 20px;
}
.button-list {
  width: 100%;
  text-align: center;

  .btn {
    border-radius: 60px;
    margin: 0 20px;
  }
}
</style>