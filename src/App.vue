<template>
  <header class="container">
    <menu-component/>
  </header>
  <poster-component
      :title="poster_title"
      :text="poster_text"
      btn_title="Sign up"
  />
  <main class="container">
    <users-list-component
        ref="reloadList"
    />
    <reg-field-component
        :signUpDone="signUpState"
        @regSuccessful="regSuccessful"
    />
  </main>
</template>

<script>
import usersListComponent from './components/usersListComponent'
import MenuComponent from "@/components/menuComponent";
import posterComponent from "@/components/posterComponent";
import RegFieldComponent from "@/components/regFieldComponent";

export default {
  name: 'App',
  components: {
    RegFieldComponent,
    MenuComponent,
    usersListComponent,
    posterComponent
  },
  data: function () {
    return {
      signUpState: false,
      poster_title: 'Test assignment for front-end developer',
      poster_text: "What defines a good front-end developer is one that has skilled knowledge of HTML, CSS, JS with a vast understanding of User design thinking as they'll be building web interfaces with accessibility in mind. They should also be excited to learn, as the world of Front-End Development keeps evolving.",
    }
  },
  methods: {
    regSuccessful() {
      this.signUpState = true
      this.$refs.reloadList.reloadList()
      console.log('regSuccessful func in APP')
    }
  }
}
</script>

<style lang="scss">
@import "assets/style.scss";

</style>
