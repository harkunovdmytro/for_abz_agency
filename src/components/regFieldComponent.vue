<template>
  <div class="reg-field">
    <template v-if="!signUpDone">

      <form @submit.prevent="handleForm">
        <h1>
          Working with POST request
        </h1>
        <div class="inputs">
          <ui-input
              placeholder="Your name"
              @value="getName"
              :error="nameValid"
          />
          <ui-input
              placeholder="Email"
              @value="getEmail"
              :error="emailValid"
          />
          <ui-input
              placeholder="Phone"
              small="+38 (XXX) XXX - XX - XX"
              @value="getPhone"
              :error="phoneValid"
          />
        </div>
        <div class="radios-container">
          <p>Select your position</p>
          <div class="radios">
            <template
                v-for="position in positions"
                :key="position.id"
            >
              <label :for="`position`+position.id" tabindex="0">
                <input type="radio"
                       tabindex="-1"
                       :id="`position`+position.id"
                       :value="position.id"
                       v-model="position_id"
                >
                <span>{{ position.name }}</span>
              </label>
            </template>
          </div>
          <div>
            <div>
              <ui-file
                  @filesGetted="previewFiles"
                  :label="file?file.name:'Upload your photo'"
                  :error="checking?fileValid:''"
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
  props: {
    signUpDone: {type: Boolean, default: false}
  },
  data: function () {
    return {
      name: '',
      email: '',
      phone: '',
      position_id: null,
      file: null,
      signupResult: false,
      positionsArray: [],
      checking: false
    }
  },
  computed: {
    areFieldsFull() {
      return (
          this.name != ''
          && this.email != ''
          && this.phone != ''
          && this.position_id != null
          && Boolean(this.file)
      )
    },
    isFormValid() {
      if (this.checking) {
        return Boolean(
            Boolean(!(this.nameValid))
            && Boolean(!this.emailValid)
            && Boolean(!this.phoneValid)
            && this.positionIDValid
            && Boolean(!this.fileValid)
        )
      } else return false
    },
    nameValid() {
      if (this.name) {
        if (this.name.length <= 2) {
          return 'User name is too short.'
        } else if (this.name.length > 60) {
          return 'User name is too long.'
        } else return ''
      } else if (this.name == '') {
        return 'Field is empty'
      } else return 'something wrong'
    },
    emailValid() {
      if (this.email && this.email.toLowerCase()
          .match(
              /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          )) {

        return ''
      } else return 'Wrong email'
    },
    phoneValid() {
      if (this.phone) {
        if (this.phone.indexOf('+380') !== 0) {
          return 'Write "+380" at the beginning.'
        } else if (this.phone.length < 13) {
          return 'Phone number is too short.'
        } else if (this.phone.length > 13) {
          return 'Phone number is too long.'
        } else {
          return ''
        }
      } else if (this.phone == '') return 'Field is empty.'
      else return 'Something wrong'
    },
    positionIDValid() {
      return this.position_id != null
    },
    fileValid() {
      if (this.file) {
        if (this.file.name) {
          if (this.file.name.includes('.jpg') || this.file.name.includes('.jpeg')) {
            return ''
          }
          return 'User photo should be jpg/jpeg image.'
        }
      } else {
        return 'File was not added.'
      }
      return 'Something is wrong'
    },
    positions() {
      return this.positionsArray
    }
  },
  mounted() {
    this.getPositions(this.positionsArray)
  },
  methods: {
    async getPositions(array) {
      await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/positions')
          .then(function (response) {
            return response.json();
          }).then(function (data) {
            if (data.success) {
              array.length = 0
              for (let i = 0; i < data.positions.length; i++) {
                array.push(data.positions[i])
              }
            }
          })
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
            // this.token = data.token
            return data.token
          });

    },
    async handleForm() {
      if (this.isFormValid) {

        const token = await this.getToken()

        let formData = new FormData();

        formData.append('position_id', this.position_id);
        formData.append('name', this.name);
        formData.append('email', this.email);
        formData.append('phone', this.phone);
        formData.append('photo', this.file);

        // fetch begin

        const fetchResult = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/users', {
          method: 'POST',
          body: formData,
          headers: {
            'Token': token, // get token with GET api/v1/token method
          },
        }).then(function (response) {
          console.log('response')
          return response.json();
        }).then(function (data) {
          if (data.success) {
            console.log('data.success: ' + data.success);
            return data
          } else {
            alert('input error :(')
            console.log(data)
            return data
          }
        }).catch(function (error) {
          console.log(error)
        })

        // fetch end

        if (await fetchResult.success) {

          console.log('success!!!')
          this.$emit('regSuccessful')
        }
        console.log(fetchResult)
        return fetchResult;
      }
    },
    nothing() {
      console.log('nothing')
    }
  },
  watch: {
    areFieldsFull(value) {
        console.log('isNoEmptyFields')
      if (value) {
        console.log('MUST BE TRUE')
        this.checking = true
      }
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

  .signUpDone {
    img {
      max-width: 100%;
      height: auto;
    }
  }
}

</style>