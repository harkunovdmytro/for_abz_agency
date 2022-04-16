<template>
  <div class="user-card">
    <div>
      <img :src="user.photo?user.photo:'../assets/photo-cover.svg'" alt="">
    </div>
    <div class="user-card-name">{{ checkLength(user.name) }}</div>
    <div>
      <div>{{ checkLength(user.position) }}</div>
      <div>{{ checkLength(user.email) }}</div>
      <div>{{ checkLength(user.phone) }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "userCardComponent",
  props: {
    user: {}
  },
  methods: {
    checkLength(text = '') {
      const newLine = text.slice(0, 32)
      if (text.length > 32) {
        return (newLine + '...')
      } else {
        return text
      }
    }
  }
}
</script>

<style lang="scss">
.user-card {
  width: auto;
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;

  &-name {
    margin: 20px 0;
  }

  img {
    border-radius: 50%;
  }
}
</style>