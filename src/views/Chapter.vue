<template>
  <div id="chapter">
    <section v-if="displayTip" id="chapter-tip">
      <div class="card">
        <div>
          <p><strong>Tip: </strong>{{ chapterData[chapterNumber].tip }}</p>
          <i @click="toggleTip" class="material-icons">close</i>
        </div>
        <div>
          <button @click="toggleTip" class="button">Dismiss</button>
        </div>
      </div>
    </section>
    <section id="chapter-task">
      <div class="card">
        <p><strong>Task:</strong> {{ chapterData[chapterNumber].task }}</p>
        <div>
          <i class="material-icons">access_time</i>
          <p class="caption">{{ chapterData[chapterNumber].time }}</p>
        </div>
      </div>
    </section>
    <section v-if="activeChat" id="chapter-chat">
      <div class="card">
        <img :src="getChatImg" alt="image of person talking">
        <div>
          <h5>{{ chapterData[chapterNumber].chat[chatIndex].who }} speaks... </h5>
          <p>{{ chapterData[chapterNumber].chat[chatIndex].body }}</p>
        </div>
      </div>
    </section>
    <section v-if="standardFormActive" class="chapter-content">
      <div :style="{ height: getContentHeight }">
        <p class="subtitle">Step 1/3</p>
        <h6>Create your free hell account</h6>
        <p>Enjoy your free first month unlimited membership with access to all pain and suffering.</p>
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
              <button id="chapter-signup-button" type="button"
                :class="{'disabled-button-chapter': signupButtonDeactivated}"
                class="button signup-popup-item" :disabled="signupButtonDeactivated">
                  <i v-if="!signupButtonDeactivated" class="material-icons">warning</i>
                {{ signupButtonDeactivated ? 'Confront Mazikeen' : 'Mazikeen got you trapped!' }}
              </button>
            </div>
          </div>
        </form>
      </div>
    </section>
    <section v-else class="chapter-content">
      <div id="bad-form" :style="{ height: getContentHeight }">
        <div><h5>Sign Up</h5></div>
        <div>
          <div>
            <label for="name">Name</label>
            <select v-model="firstname" @change="checkRequiredFieldsBadForm" name="name" id="name">
              <option value=""></option>
              <option value="1">Roger</option>
              <option value="2">Scott</option>
              <option value="3">Jill</option>
              <option value="4">Trevor</option>
              <option value="5">{{ getName }}</option>
              <option value="6">Susan</option>
              <option value="7">Sandra</option>
              <option value="8">Andy</option>
              <option value="9">Julia</option>
              <option value="10">Andrew</option>
            </select>
          </div>
          <div>
            <label for="surname">Surname</label>
            <input v-model="surname" type="text" name="surname" disabled>
            <div id="surname-controls">
              <button @click="shiftCharacterIndex('left')" class="button button-small"><i class="material-icons">chevron_left</i></button>
              <button @click="shiftCharacterIndex('right')" class="button button-small"><i class="material-icons">chevron_right</i></button>
              <button @click="setCharacter" class="button button-small"><i class="material-icons">check</i></button>
              <button @click="clearSurname" class="button button-small"><i class="material-icons">close</i></button>
            </div>
            <div id="alphabet">
              <p v-for="(char, index) in alphabet"
                :index="index"
                :key="index"
                :class="{ 'character-border': characterIndex === index }">{{ char }}</p>
            </div>
          </div>
        </div>
        <div>
          <div id="gender">
            <div>
              <input @click="gameOfChance('female')" type="radio" id="radio-male" name="gender" value="0">
              <label for="male">Male</label>
            </div>
            <div>
              <input @click="gameOfChance('male')" type="radio" id="radio-female" name="gender" value="1">
              <label for="female">Female</label>
            </div>
          </div>
        </div>
        <div>
          <div>
            <label for="generate-email">Email</label>
            <input v-model="email" type="text" name="generate-email" disabled>
            <div id="email-controls">
              <button @click="getRandomEmail" class="button button-medium">Generate</button>
            </div>
          </div>
        </div>
        <div>
          <div>
            <label for="set-password">Password</label>
            <input v-model="generatedPassword" @input="checkRequiredFieldsBadForm" type="text" disabled>
            <div id="password-controls-one">
              <button @click="togglePasswordGeneration" class="button button-medium">{{ passwordGenerationInterval === undefined ? 'start' : 'stop' }}</button>
            </div>
          </div>
        </div>
        <div>
          <div>
            <button v-if="!finishButtonActive" @click="showEducationalContent" class="button" :class="{'disabled-button': attackButtonDeactivated}" :disabled="attackButtonDeactivated">Attack Mazikeen</button>
            <router-link class="button" v-else tag="button" to="/dashboard">FINISH CHAPTER</router-link>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { setTimeout, setInterval, clearInterval } from 'timers'
export default {
  props: ['chapterNumber'],
  data () {
    return {
      displayTip: false,
      tipTimer: 15,
      contentHeight: '93%',
      activeChat: false,
      chatIndex: 0,
      signupButtonDeactivated: true,
      name: '',
      firstname: -1,
      surname: '',
      password: '',
      generatedPassword: '',
      email: '',
      nameLabelFocus: false,
      passwordLabelFocus: false,
      standardFormActive: true,
      alphabet: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
      characterIndex: 0,
      listOfEmails: ['mranderson@thematrix.com', 'stevebrown@theshack.net', 'order@subway.com', 'stevenrogers@teamamerica.com', 'michaelscott@dundermifflin.com'],
      passwordGenerationInterval: undefined,
      listOfPasswords: ['password', '12345', 'thebestpassword', 'lucifer', 'theoffice', 'toaster', 'flamingo', 'redacted', 'straya', 'mypetname', 'brisbane'],
      attackButtonDeactivated: true,
      finishButtonActive: false
    }
  },
  methods: {
    toggleTip () {
      this.displayTip = !this.displayTip
      this.displayTip ? this.contentHeight = '93%' : this.contentHeight = '94%'
    },
    checkRequiredFieldsBadForm () {
      if (this.firstname == 5 && this.surname !== '' && this.email == this.getName + '@gmail.com' && this.generatedPassword == this.getPassword) {
        this.attackButtonDeactivated = false
      } else {
        this.attackButtonDeactivated = true
      }
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
    },
    shiftCharacterIndex (direction) {
      direction === 'left' ? this.characterIndex-- : this.characterIndex++
      if (this.characterIndex < 0) {
        this.characterIndex = 27
      } else if (this.characterIndex > 27) {
        this.characterIndex = 0
      }
    },
    setCharacter () {
      this.surname = this.surname + this.alphabet[this.characterIndex]
      this.checkRequiredFieldsBadForm()
    },
    clearSurname () {
      this.surname = ''
    },
    gameOfChance (gender) {
      const rand = Math.random()
      if (rand < 0.4) {
        setTimeout(function () {
          clearInterval(interval)
        }, 2000)
        const interval = setInterval(function () {
          gender === 'male' ? gender = 'female' : gender = 'male'
          document.getElementById('radio-' + gender).checked = true
        }, 300)
      } else if (rand < 0.8) {
        setTimeout(function () {
          document.getElementById('radio-' + gender).checked = true
        }, 300)
      }
    },
    getRandomEmail () {
      let rand = Math.floor(Math.random() * this.listOfEmails.length)
      while (this.email === this.listOfEmails[rand]) {
        rand = Math.floor(Math.random() * this.listOfEmails.length)
      }
      this.email = this.listOfEmails[rand]
      this.checkRequiredFieldsBadForm()
    },
    togglePasswordGeneration () {
      if (this.passwordGenerationInterval === undefined) {
        const that = this
        this.passwordGenerationInterval = setInterval(function () {
          let rand = Math.floor(Math.random() * that.listOfPasswords.length)
          that.generatedPassword = that.listOfPasswords[rand]
          that.checkRequiredFieldsBadForm()
        }, 300)
      } else {
        clearInterval(this.passwordGenerationInterval)
        this.passwordGenerationInterval = undefined
      }
    },
    showEducationalContent () {
      const that = this
      that.chatIndex++
      that.activeChat = true
      setTimeout(function () {
        that.chatIndex++
        setTimeout(function () {
          that.chatIndex++
          setTimeout(function () {
            that.chatIndex++
            setTimeout(function () {
              that.chatIndex++
              setTimeout(function () {
                that.chatIndex++
                setTimeout(function () {
                  that.chatIndex++
                  setTimeout(function () {
                    that.activeChat = false
                    that.finishButtonActive = true
                  }, 8000)
                }, 8000)
              }, 8000)
            }, 8000)
          }, 8000)
        }, 8000)
      }, 8000)
    }
  },
  computed: {
    chapterData () {
      return this.$store.getters.getChapterData
    },
    getContentHeight () {
      return this.contentHeight
    },
    getChatImg () {
      return require('../assets/' + this.chapterData[this.chapterNumber].chat[this.chatIndex].img + '.png')
    },
    getTipTimer () {
      return this.tipTimer
    },
    getName () {
      return this.$store.getters.getName
    },
    getPassword () {
      return this.$store.getters.getPassword
    }
  },
  mounted () {
    this.listOfEmails.push(this.getName + '@gmail.com')
    this.listOfPasswords.push(this.getPassword)

    const that = this
    setTimeout(function () {
      that.activeChat = true
      setTimeout(function () {
        that.chatIndex++
        setTimeout(function () {
          that.chatIndex++
          setTimeout(function () {
            that.chatIndex++
            setTimeout(function () {
              that.chatIndex++
              setTimeout(function () {
                that.chatIndex++
                setTimeout(function () {
                  that.chatIndex++
                  setTimeout(function () {
                    that.chatIndex++
                    that.standardFormActive = false
                    setTimeout(function () {
                      that.activeChat = false
                      setTimeout(function () {
                        that.displayTip = true
                        setTimeout(function () {
                          that.displayTip = false
                        }, 10000)
                      }, 2000)
                    }, 5000)
                  }, 5000)
                }, 5000)
              }, 5000)
            }, 5000)
          }, 15000)
        }, 5000)
      }, 5000)
    }, 2000)
  }
}
</script>

<style>
#chapter {
  display: flex;
  flex-direction: column;
}
#chapter-tip > .card {
  height: calc(auto + 1rem);
}
#chapter-tip > .card > div:first-of-type {
  display: flex;
  justify-content: space-between;
}
#chapter-tip > .card > div > i {
  margin: 1rem 1rem 0 0;
  cursor: pointer;
}
#chapter-tip > .card > div:last-of-type {
  margin-bottom: 1rem;
}
#chapter-tip > .card > div:last-of-type > .button {
  margin-left: 1rem;
}
#chapter-task > .card {
  height: 5vh;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
#chapter-task > .card > div {
  display: flex;
  padding: 0 1em 0 0.4em;
}
#chapter-task > .card > div > p {
  margin: 6px 0 0 0;
}
#chapter-task > .card > p, #chapter-tip > .card > div > p {
  padding: 0 0 0 1em;
}
#chapter-chat > div {
  display: flex;
  justify-content: flex-start;
  flex-direction: row;
}
#chapter-chat > div > img {
  height: 150px;
}
#chapter-chat > div > div {
  display: flex;
  flex-direction: column;
  padding: 0 1em 0 1em;
}
#chapter-chat > div > div > h5 {
  margin: 1rem 0 0 0;
}
.chapter-content {
  flex: 1 1 auto;
}
.chapter-content > div {
  height: 93%;
  margin: auto;
  width: 30%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.chapter-content > div > h6 {
  margin: 0.5rem 0 0.5rem 0;
}
.disabled-button-chapter {
  background-color: #676565;
  display: inline-block !important;
  align-items: none;
}
#chapter-signup-button {
  background-color: #676565;
  display: flex;
  align-items: center;
  justify-content: center;
}
/* #chapter-signup-button > i {
  padding-left: 0;
} */
#bad-form {
  width: 60%;
  display: flex;
  flex-direction: column;
}
#bad-form > div {
  display: flex;
  flex-direction: row;
}
#bad-form > div > div {
  width: 50%;
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}
#name {
   overflow-y: scroll;
}
select option {
  height: 10px;
}
#surname-controls, #email-controls, #password-controls-one {
  position: absolute;
  align-self: flex-end;
  padding: 1.5em 0.5em 1.5em 0;
}
#alphabet > p {
  display: inline;
}
#gender {
  flex-direction: row !important;
  justify-content: space-evenly;
}
.character-border {
  border: 1px solid #D52916;
}
</style>
