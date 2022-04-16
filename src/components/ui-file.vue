<template>
  <div class="file">
    <label>
      <input id="fileField" @change="previewFiles" type="file">
      <button onclick="document.getElementById('fileField').click()">Upload</button>
      <span>{{ label }}</span>
    </label>

  </div>
</template>

<script>
export default {
  name: "ui-file",
  props: {
    label: {type: String, default: 'Upload your photo'},
  },
  methods: {
    previewFiles(event) {
      this.$emit('filesGetted', event.target.files)
    }
  }
}
</script>

<style lang="scss">
.file {
  margin: 47px auto 50px;

  label {
    display: flex !important;
    justify-content: space-between;
    width: 100%;
    cursor: pointer;

    button {
      display: block;
      border-radius: 4px 0 0 4px;
      border: 1px #000 solid;
      background-color: inherit;
    }

    button, span {
      padding: 15px;
    }

    input[type='file'] {
      display: none
    }

    span {
      color: #7E7E7E;
      width: 100%;
      border: 1px #D0CFCF solid;
      border-radius: 0 4px 4px 0;
    }
  }
}
</style>