<template>
    <div id="home">
      <div v-if="displayPopup" class="sign-up-popup">
        <div @click="togglePopup" class="popup-backdrop"></div>
        <div class="card card-popup">
          <div>
            <h4>Create account</h4>
            <p>Hi there! We hate paperwork too, so let's keep it short and friendly</p>
            <form class="signup-form">
              <div class="floating-input">
                <label :class="{'float-label': nameLabelFocus}" class="subtitle-2" for="username">Name</label>
                <input @focus="labelFocus('name')" @blur="labelFocus('name')" v-model="name" @input="checkRequiredFields" class="signup-popup-item" type="text" name="name" id="username">
              </div>
              <div class="floating-input">
                <label :class="{'float-label': passwordLabelFocus}" class="subtitle-2" for="userpassword">Password</label>
                <input @focus="labelFocus('password')" @blur="labelFocus('password')" v-model="password" @input="checkRequiredFields" class="signup-popup-item" type="password" name="password" id="userpassword">
              </div>
              <div class="button-row">
                <div>
                  <button type="button" @click="authenticateUser" :class="{'disabled-button': signupButtonDeactivated}" class="button signup-popup-item" :disabled="signupButtonDeactivated">Go to Dashboard</button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
      <section>
        <div>
          <header>
            <h3 class="page-header-tagline">The Devil Teaches Bad Web Design</h3>
          </header>
          <p>Become a expert web designer by learning what makes
            <br>
            for a horrible experience from the Devil himself!</p>
          <div class="button-row">
            <button @click="checkIfAuthenticated" class="button">
            get started
            </button>
          </div>
        </div>
      </section>
    </div>
</template>

<script>
export default {
  data () {
    return {
      displayPopup: false,
      signupButtonDeactivated: true,
      name: '',
      password: '',
      nameLabelFocus: false,
      passwordLabelFocus: false
    }
  },
  methods: {
    togglePopup () {
      this.displayPopup = !this.displayPopup
      this.displayPopup ? document.body.style.overflow = 'hidden' : document.body.style.overflow = 'visible'
    },
    checkIfAuthenticated () {
      if (this.$store.getters.getAuthStatus) {
        this.authenticateUser()
      } else {
        this.togglePopup()
      }
    },
    authenticateUser () {
      this.$store.dispatch('setName', this.name.toLowerCase())
      this.$store.dispatch('setPassword', this.password.toLowerCase())
      this.$store.dispatch('authenticate', true)
      this.togglePopup()
      this.$router.push('/dashboard')
    },
    checkRequiredFields () {
      if (this.name !== '' && this.password !== '') {
        this.signupButtonDeactivated = false
      } else {
        this.signupButtonDeactivated = true
      }
    },
    labelFocus (element) {
      if (element === 'name') {
        if (this.name === '') {
          this.nameLabelFocus = !this.nameLabelFocus
        }
      } else {
        if (this.password === '') {
          this.passwordLabelFocus = !this.passwordLabelFocus
        }
      }
    }
  }
}
</script>

<style>
#home {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
#home > section, #about > section {
  min-height: 100vh;
  display: flex;
}
#home > section:not(:first-of-type) > div {
  width: 75%;
}
#home > section > div, #about > section > div {
  margin: auto;
}
.page-header-tagline {
  margin-block-end: 0.5em;
}
.selling-points-container {
  display: flex;
  justify-content: space-between;
}
.selling-points {
  flex-basis: 30%;
  background-color: #676565;
  height: 300px;
}
.sign-up-popup {
  position: fixed;
  display: flex;
  width: 100%;
  height: 100%;
  z-index: 1000;
  top: 0;
  left: 0;
}
.popup-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,.4);
}
.card-popup {
  margin: auto;
  display: flex;
  width: 30%;
  height: 50%;
}
.card-popup > div {
  margin: auto;
  width: 60%;
}
.card-popup > div > h4 {
  margin: 0;
}
.signup-form {
  display: flex;
  flex-direction: column;
}
.floating-input label {
  position: absolute;
  padding: 1em;
  color: rgba(255, 255, 255, .7)
}
.float-label {
  padding: 0.1em 1em 1em 1.5em !important;
  font-size: 0.6rem;
}
.signup-popup-item {
  width: 100%;
  box-sizing: border-box;
}
</style>
