<template>
  <div class="file" :class="errorCheck">
    <label>
      <input id="fileField" @change="previewFiles" type="file">
      <button onclick="document.getElementById('fileField').click()">Upload</button>
      <span>{{ label }}</span>
    </label>
    <template
    v-if="errorMessage"
    >
      <small class="errorMessage">{{errorMessage}}</small>
    </template>
    <template
    v-if="error"
    >
      <small class="errorMessage">{{error}}</small>
    </template>
  </div>
</template>

<script>
export default {
  name: "ui-file",
  props: {
    label: {type: String, default: 'Upload your photo'},
    error: {type: String, default: ''},
  },
  data: function () {
    return {
      isError: false,
      errorMessage: '',
      fileMaxSizeBytes:5000000 //5Mb
    }
  },
  methods: {
    previewFiles(event) {
      console.log(event.target.files[0])
      if (event.target.files[0].size > this.fileMaxSize) {
        this.isError = true
        this.errorMessage = 'File is too big. Choose image less then 5Mb'
      } else {
        this.isError = false
        this.errorMessage = ''
        this.$emit('filesGetted', event.target.files)
      }
    },
  },
  computed: {
    errorCheck() {
      if (this.isError||this.error) return 'error'
      else return ''
    }
  }
}
</script>

<style lang="scss">
.file {
  margin: 47px auto 50px;

  &.error span {
    border-top: 2px solid #CB3D40;
    border-right: 2px solid #CB3D40;
    border-bottom: 2px solid #CB3D40;
  }

  &.error button {
    border: 2px solid #CB3D40 !important;
  }

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

.errorMessage {
  margin-left: 16px;
  color: #CB3D40 !important;
}
</style>