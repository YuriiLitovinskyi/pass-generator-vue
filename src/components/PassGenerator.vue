<template>
  <div>
    <h1>{{ msg }}</h1>
    <div class="container">
      <form @submit.prevent="generatePass" class="genForm">
        <div class="param">
          <label for="passGen">Enter number of characters: </label>
          <input type="number" id="passGen" placeholder="Enter number" v-model="numberOfChars" required>
        </div>
        <hr />

        <div class="param">
          <label for="numbers">Use numbers (0-9)</label>
          <input type="checkbox" name="numbers" id="numbers" v-model="numbers">
        </div>
        <hr />

        <div class="param">
          <label for="lowLetters">Use lowercase letters (a-z)</label>
          <input type="checkbox" name="lowLetters" id="lowLetters" v-model="lowLetters">
        </div>
        <hr />

        <div class="param">
          <label for="upLetters">Use uppercase letters (A-Z)</label>
          <input type="checkbox" name="upLetters" id="upLetters" v-model="upLetters">
        </div>
        <hr />

        <div class="param">
          <label for="symbols">Use special symbols (@, %, ets)</label>
          <input type="checkbox" name="symbols" id="symbols" v-model="symbols">
        </div>
        <hr />

        <input type="submit" value="Generate">

      </form>

    </div>

    <div v-show="password" class="output-container">
      <div @click.stop.prevent="copyPassword" class="output">{{ password }}</div>
      <input type="hidden" id="copyPwd" :value="password">
    </div>

    <Alert v-show="showAlert" v-bind:alertMessage="alertMessage"/>
       
  </div>
</template>

<script>
import Alert from '@/components/Alert'

export default {
  name: 'HelloWorld',
  components: {
    Alert
  },
  props: {
    msg: String
  },
  data(){
    return {
      numberOfChars: 6,
      numbers: true,
      lowLetters: false,
      upLetters: false,
      symbols: false,
      password: '',
      alertMessage: '',
      showAlert: false
    }
  },
  methods: {   
    randomChars(chars){
      return chars.charAt(Math.floor(Math.random() * chars.length))
    },
    generateRandomPass(){ 

      let conditions = [this.numbers, this.lowLetters, this.upLetters,this.symbols]
      let bits = conditions.reduce( (accum,current,index) => accum+(current<<index), 0);    
      
      switch(bits){
        case 0:
          return 'x'
        case 1:
          return this.randomChars('0123456789')
        case 2:
          return this.randomChars('abcdefghijklmnopqrstuvwxyz')
        case 3:
          return this.randomChars('0123456789abcdefghijklmnopqrstuvwxyz')
        case 4:
          return this.randomChars('ABCDEFGHIJKLMNOPQRSTUVWXYZ')
        case 5:
          return this.randomChars('0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ')
        case 6:
          return this.randomChars('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ')
        case 7:
          return this.randomChars('0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ')
        case 8:
          return this.randomChars('!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 9:
          return this.randomChars('0123456789!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 10:
          return this.randomChars('abcdefghijklmnopqrstuvwxyz!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 11:
          return this.randomChars('0123456789abcdefghijklmnopqrstuvwxyz!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 12:
          return this.randomChars('ABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 13:
          return this.randomChars('0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 14:
          return this.randomChars('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
        case 15:
          return this.randomChars('0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~')
      }
    }, 
    generatePass(){  
      this.password = ''
      this.alertMessage = ''
      this.showAlert = false

      if(this.numberOfChars <= 0){
        this.password = 'ERROR'
        return
      }

      for(let i = 0; i < this.numberOfChars; i++){
        this.password += this.generateRandomPass()
      }
    },
    copyPassword(){
      let pwd = document.querySelector('#copyPwd')
      pwd.setAttribute('type', 'text')
      pwd.select()


      try {      
        if(document.execCommand('copy')){
          this.alertMessage = 'Password was copied'
          this.showAlertMessage(2000)
        }
      } catch (error) {      
        this.alertMessage = 'Could not copy password!'
        this.showAlertMessage(2000)
      }

      pwd.setAttribute('type', 'hidden')
      window.getSelection().removeAllRanges()
    },
    showAlertMessage(secs){
      this.showAlert = true
      setTimeout(() => {
        this.showAlert = false
      }, secs)
    }
  }
}
</script>

<style scoped>
.container, .output-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0; 
}
.genForm {
  min-width: 30vw;
  background-color: #D3D3D3;
  margin: 5px;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 3px 3px 5px 10px #F0F0F0;
}
.param {
  margin: 5px;
  padding: 5px;
  display: flex;
  justify-content: space-between;
}
.param input {
  border: 2px solid #f0f0f0;
  border-radius: 5px;  
  width: 10%;
  padding: 3px;
  font-size: 14px;
  color: #777;
  font-weight: bold;
}
input[type="submit"] {
  cursor: pointer;
  background-color: #3498db;
  border: 2px solid #3498db;
  border-radius: 4px;
  color: #fff;
  display: block;
  font-size: 16px;
  padding: 8px;
  width: 100%; 
  margin-top: 20px;
  text-transform: uppercase;
}
input[type="submit"]:hover {
  background-color: #348adb;
  border-color: #348adb; 
}
input[type="submit"]:active {
  transform: scale(1.01);
}
.output {
  margin-top: 20px;
  font-weight: bold;
  font-size: 1.5rem;
  letter-spacing: 2px;
  border: 2px solid black;
  border-radius: 6px;
  padding: 5px;
}
.output:hover {
  background-color: #777;
  color: #fff;
  cursor: pointer;
}
</style>
