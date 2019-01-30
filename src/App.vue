<template>
  <div id="app">
    <p>Input Value: {{chatInput}}</p>
   <xf-input :chatInput.sync="chatInput" :users="users" @btnClick="hdlReply" @itemSelected="hdlSelected"/>
  </div>
</template>

<script>
import XfInput from './components/XfInput.vue'
import users from '@/assets/users.js'

export default {
  name: 'app',
  components: {
    XfInput
  },
  data(){
    return{
      chatInput: '',
      users: users,
    }
  },
  methods:{
    hdlReply(){
    },
    hdlSelected(v){
      console.log(JSON.parse(JSON.stringify(v)))
      // let replaceInput = this.chatInput.split("@")[0] +' ' + JSON.parse(JSON.stringify(this.matches[this.selected])).name
      //reparse object since its on Observer
      let newInput = this.chatInput.split("@")[0] + ' ' + JSON.parse(JSON.stringify(v)).name
      this.chatInput = newInput
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
