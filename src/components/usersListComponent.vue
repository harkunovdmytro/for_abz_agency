<template>
  <div class="user-list-container">
    <h1>Working with GET request</h1>
    <div class="user-list">
      <template
          v-for="user in userData"
          :key="user.id+userData.length"
      >
        <user-card-component :user="user"/>
      </template>
    </div>
    <div class="show-more-btn" v-show="!showBtnHiden">
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
      showBtnHiden: false,
      calling_index: 1,
      usersArray: [],
      sorted: []
    }
  },
  mounted() {
    this.loadUsers()
    this.fetchData()
  },
  computed: {
    userData() {

      // registration_timestamp
      return this.usersArray
    },

  },
  methods: {
    reloadList() {
      this.usersArray.length = 0
      this.calling_index = 1
      this.fetchData()
    },
    async loadUsers() {
      const USERS_LOADED = 6
      const load = await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.calling_index}&count=${USERS_LOADED}`)
      const response = await load.json()
      return response.users
    },
    async fetchData() {
      const USERS_LOADED = 6

      return await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.calling_index}&count=${USERS_LOADED}`)
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            if (!data.message) {
              for (let i = 0; i < data.users.length; i++) {
                this.usersArray.push(data.users[i])
              }
              this.usersArray.sort((a, b) => a.registration_timestamp < b.registration_timestamp ? 1 : -1)
              this.calling_index++
            } else {
              this.showBtnHiden = true

            }
          }).catch((error) => {
            console.log(error)
            this.showBtnHiden = true
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
  .user-list {
    grid-template-columns: repeat(2, 1fr) !important;
  }
}

@media (min-width: 1023px) {
  .user-list {
    grid-template-columns: repeat(3, 1fr) !important;
  }
}
</style>