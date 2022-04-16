<template>
  <div class="input">
    <div class="input-field">
      <input :class="inputClasses"
             type="text"
             v-model="value"
             @keypress="passValue"
             @focusin="isFocused = true"
             @focusout="isFocused = false"
      >

      <template v-if="placeholder">
        <span :class="enterError" class="label">{{ placeholder }}</span>
      </template>

    </div>
    <template v-if="small">
      <div>
        <small class="input-assist">{{ small }}</small>
      </div>
    </template>
    <template v-if="error">
      <div>
        <small :class="enterError">{{ error }}</small>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "inputComponent",
  props: {
    placeholder: {type: String, default: ''},
    isPass: {type: Boolean, default: false},
    small: {type: String, default: ''},
    error: {type: String, default: ''},
  },
  data: function () {
    return {
      value: '',
      isFocused: false,
    }
  },
  computed: {
    inputClasses() {
      let inputClass = ''

      if (this.hasValue) inputClass += 'not-empty '
      if (this.isFocused) inputClass += 'focused '
      if (this.error) inputClass += 'error '
      this.$emit('value', this.value)
      return inputClass
    },
    enterError() {
      if (this.error) return 'error'
      return ''
    },
    hasValue() {
      if (this.value) return true
      else return false
    }
  },
  methods: {

  }
}
</script>

<style lang="scss">
@import "../assets/style.scss";


.input {
  .input-assist {
    position: relative;
    left: 16px;
  }

  .input-field {
    position: relative;

    input {
      width: 100%;
      background-color: inherit;
      border: solid #D0CFCF 1px;
      border-radius: 4px;
      outline: none;
      padding: 16px;

      &.error {
        border: solid $error-color 1px !important;
      }
    }

    input.not-empty + .label,
    input.focused + .label,
    input.not-empty.focused + .label,
    {
      top: 0 !important;
    }
  }
  span.error {
    color: $error-color !important;
  }
  input.error {
    border-color: $error-color !important;
  }
  .label {
    font-size: 16px;
    transition: 0.3s;
    position: absolute;
    transform: translate(0, -50%);
    top: 50%;
    left: 16px;
    background-color: $background-color;
    padding-right: 5px;
    color: #7E7E7E;
  }
}
small.error {
  color: $error-color;
}
</style>