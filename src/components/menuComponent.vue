<template>
  <nav class="menu">
    <div><a href="#"><img src="../assets/logo.svg" alt=""></a></div>
    <div class="menu-links">
      <btn-component title="Users"/>
      <btn-component title="Sign up"/>
    </div>
  </nav>
</template>

<script>
import btnComponent from './ui-btn'

export default {
  name: 'menuComponent',
  components: {
    btnComponent,
  }
}
</script>
<style lang="scss">
.menu {
  padding: 13px 15px;
  display: flex;
  justify-content: space-between;
  align-content: center;
  &-links > *:not(:last-child) {
    margin-right: 10px;
  }
}
</style>