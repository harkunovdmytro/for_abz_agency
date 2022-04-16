<template>
  <div class="reg-field">
    <template v-if="!signUpDone">

      <form @submit.prevent="signUpDone = handleForm">
        <h1>
          Working with POST request
        </h1>
        <div class="inputs">
          <ui-input
              placeholder="Your name"
              @value="getName"
              :error="positionIDValid?nameValid:''"
          />
          <ui-input
              placeholder="Email"
              @value="getEmail"
              :error="positionIDValid?emailValid:''"
          />
          <ui-input
              placeholder="Phone"
              small="+38 (XXX) XXX - XX - XX"
              @value="getPhone"
              :error="positionIDValid?phoneValid:''"
          />
        </div>
        <div class="radios-container">
          <p>Select your position</p>
          <div class="radios">
            <label>
              <input type="radio" v-model="position_id" value="1">
              <span>Frontend developer</span>
            </label>
            <label>
              <input type="radio" v-model="position_id" value="2">
              <span>Backend developer</span>
            </label>
            <label>
              <input type="radio" v-model="position_id" value="3">
              <span>Designer</span>
            </label>
            <label>
              <input type="radio" v-model="position_id" value="4">
              <span>QA</span>
            </label>
          </div>
          <div>
            <div>
              <ui-file
                  @filesGetted="previewFiles"
                  :label="file?file.name:'Upload your photo'"
              />
            </div>
          </div>
          <div class="form-btn">
            <ui-btn
                title="Sign up"
                :disabled="!isFormValid"
                @btn-clicked="nothing"

            />
          </div>
        </div>
      </form>
    </template>
    <template v-if="signUpDone">

      <div class="signUpDone">
        <h1>You have successfully signed up.</h1>
        <div>
          <img src="../assets/success-image.svg" alt="You have successfully signed up.">

        </div>
      </div>
    </template>
  </div>
</template>

<script>

import uiInput from "@/components/ui-input";
import UiFile from "@/components/ui-file";
import UiBtn from "@/components/ui-btn";

export default {
  name: "regFieldComponent",
  components: {UiFile, uiInput, UiBtn},
  data: function () {
    return {
      name: '',
      email: '',
      phone: '',
      position_id: null,
      token: '',
      file: {},
      fails: [],
      signUpDone: false
    }
  },
  computed: {
    isFormValid() {
      return (
          this.nameValid == ''
          && this.emailValid == ''
          && this.phoneValid == ''
          && this.positionIDValid
          && this.fileValid)
    },
    nameValid() {
      if (this.name.length <= 2) {
        return 'User name is too short.'
      } else if (this.name.length > 60) {
        return 'User name is too long.'
      } else return ''
    },
    emailValid() {
      if (this.email.indexOf('@') < this.email.indexOf('.') && this.email.includes('@') && this.email.includes('.')) return ''
      else return 'Email is not valid.'
    },
    phoneValid() {
      if (this.phone.indexOf('+38') !== 0) {
        return 'Write "+38" at the beginning.'
      } else if (this.phone.length < 13) {
        return 'Phone number is too short.'
      } else if (this.phone.length > 13) {
        return 'Phone number is too long.'
      } else {
        return ''
      }
    },
    positionIDValid() {
      return this.position_id != null
    },
    fileValid() {
      return this.file.name ? true : false
    },
  },
  mounted() {
    this.getToken()
  },
  methods: {
    changeSignUpDone() {
      this.signUpDone = true
    },
    previewFiles(files) {
      this.file = {}
      if (files.length > 1) {
        alert("Too many files. Load only one photo.")
      } else {
        this.file = files[0]
      }
    },
    getName(inputValue) {
      this.name = inputValue
    },
    getEmail(inputValue) {
      this.email = inputValue
    },
    getPhone(inputValue) {
      this.phone = inputValue
    },
    getToken() {
      return fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/token`)
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            this.token = data.token
          });

    },
    handleForm() {
      if (this.isFormValid) {
        let formData = new FormData(); // file from input type='file'

        formData.append('position_id', this.position_id);
        formData.append('name', this.name);
        formData.append('email', this.email);
        formData.append('phone', this.phone);
        formData.append('photo', this.file);
        console.log(formData)
        return fetch('https://frontend-test-assignment-api.abz.agency/api/v1/users', {
          method: 'POST', body: formData, headers: {
            'Token': this.token, // get token with GET api/v1/token method
          },
        })
            .then(function (response) {
              return response.json();
            })
            .then(function (data) {
              if (data.success) {
                console.log('data.success: ' + data.success);
              } else {
                alert('input error :(')
                console.log(data)
              }
            })
            .catch(function (error) {
              console.log(error)
            });
      }
    },
    nothing() {
      console.log('nothing')
    }
  }
}
</script>

<style lang="scss">
.reg-field {
  width: min(380px, 100%);
  padding: 15px;
  margin: 140px auto 100px;

  .form-btn {
    text-align: center;
    margin-top: 50px;
  }

  h1 {
    text-align: center;
  }

  .inputs {
    > * {
      margin: 50px 0;
    }
  }

  .radios-container {
    margin: 25px auto 50px;

    input[type='radio'] {
      width: 20px;
      height: 20px;
    }

    input[type='radio'] {
      opacity: 0;
    }

    input[type='radio'] + span {
      &:before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        background-color: inherit;
        border: 1px solid #D0CFCF;
        width: 18px;
        height: 18px;
        border-radius: 50%;
      }
    }

    input[type='radio']:checked {
      opacity: 0;
    }

    input[type='radio']:checked + span {
      &:before {
        position: absolute;
        top: 0;
        left: 0;
        content: '';
        background-color: inherit;
        //background-color: red !important;
        border: 1px solid #00BDD3;
        width: 18px;
        height: 18px;
        border-radius: 50%;
      }

      &:after {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        background-color: #00BDD3;
        transform: translate(5px, 5px);
        width: 10px;
        height: 10px;
        border: none;
        border-radius: 50%;
      }
    }

    .radios {
      margin: 10px auto;
      display: flex;
      flex-flow: column;

      span {
        margin-left: 12px;
      }

      label {
        position: relative;
        display: flex;
        align-content: center;
        margin-top: 7px;
        cursor: pointer;
      }
    }
  }
}

</style>