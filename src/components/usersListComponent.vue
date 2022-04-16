<template>
  <div class="user-list-container">
    <h1>Working with GET request</h1>
    <div class="user-list">
      <template
          v-for="user in userData"
          :key="user.id"
      >
        <user-card-component :user="user"/>
      </template>
    </div>
    <div class="show-more-btn">
      <btn-component @btn-clicked="fetchData" title="Show more"/>
    </div>
  </div>
</template>

<script>
import userCardComponent from "@/components/userCardComponent";
import BtnComponent from "@/components/ui-btn";

export default {

  name: "usersListComponent",
  components: {BtnComponent, userCardComponent},
  data: function () {
    return {
      calling_index: 1,
      myusers: [],
    }
  },
  mounted() {
    this.users = this.loadUsers()
    this.fetchData()
  },
  computed: {
    userData() {
      return this.myusers
    }
  },
  methods: {
    async loadUsers() {
      const USERS_LOADED = 6
      const load = await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.calling_index}&count=${USERS_LOADED}`)
      const response = await load.json()
      return response.users
    },
    fetchData() {
      const USERS_LOADED = 6

      return fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.calling_index}&count=${USERS_LOADED}`)
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            for (let i = 0; i < data.users.length; i++) {
              this.myusers.push(data.users[i])
            }
            this.calling_index++
          });
    }
  }
}
</script>

<style lang="scss">
@import "../assets/style.scss";

.user-list-container {
  margin: 140px auto;

  h1 {
    text-align: center;
    margin-bottom: 50px;
  }

  .user-list {
    display: grid;
    grid-auto-flow: row;
    grid-gap: 10px !important;
    grid-template-columns: repeat(1, 1fr);
  }
}
@media (min-width: 767px) {
  .user-list{
    grid-template-columns: repeat(2, 1fr) !important;
  }
}
@media (min-width: 1023px) {
  .user-list{
    grid-template-columns: repeat(3, 1fr) !important;
  }
}
</style>