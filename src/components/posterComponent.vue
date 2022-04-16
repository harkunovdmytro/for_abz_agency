<template>
  <div class="poster">
    <div class="poster-content">
      <h1>{{ title }}</h1>
      <p>{{ text }}</p>
      <div>
        <btn-component
            :title="btn_title"
            :href="btn_link"
        />
      </div>
    </div>
  </div>
</template>
<script>
import btnComponent from "@/components/ui-btn";

export default {
  name: 'posterComponent',
  components: {
    btnComponent
  },

  props: {
    title: {type: String, default: ''},
    text: {type: String, default: ''},
    btn_title: {type: String, default: 'Click'},
    btn_link: {type: String, default: '#'},
  }
}
</script>
<style lang="scss">
.poster {
  &-content {
    margin: auto;
    width: min(380px, 100%);
    min-height: 100vh;
    display: flex;
    flex-flow: column;
    justify-content: center;
    text-align: center;
    padding: 10px;
  }

  background-image: url("../assets/pexels-alexandr-podvalny-1227513.jpeg");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;

  *:not(.btn) {
    color: #fff !important;
  }

  .btn {
    margin-top: 32px;
  }
}
</style>