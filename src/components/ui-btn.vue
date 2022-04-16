<template>
  <button
      :href="href"
      class="btn"
      :disabled="isDisabled"
      @click="(href != '#')?moveTo:this.$emit('btn-clicked')"
  >
    {{ title }}
  </button>
</template>

<script>
export default {
  name: 'btnComponent',
  props: {
    isDisabled: {type: Boolean, default: false},
    title: {type: String, default: 'Click'},
    href: {type: String, default: '#'}
  },
  methods: {
    moveTo() {
      console.log('moveTo rule:' + (this.href != '#'))
      if (this.href != '#') {
        console.log('href rule clicked')
        window.open(this.href, '_self')
      } else {
        console.log('button clicked')
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "../assets/style.scss";

.btn {
  background-color: $primary-color;
  color: $font-color;
  border-radius: 100px;
  padding: 4px 20px;
  border: none;
  outline: none;
  cursor: pointer;

  &:hover {
    background-color: $btn-hover;
  }

  &:disabled {
    cursor: default;
    background-color: $btn-disabled;
    color: $font-color-btn-disabled;
  }
}
</style>
