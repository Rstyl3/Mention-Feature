<template>
  <div class="x-input">
    <input
      type="text"
      ref="input"
      :value="chatInput"
      @input="$emit('update:chatInput', $event.target.value)"
      @keyup.enter="$emit('btnClick')"
      @keydown.up="up"
      @keydown.down="down"
      @keydown.enter="selectItem"
      placeholder="Type @ to mention someone"
    >
    <button @click="$emit('btnClick')">Send</button>
    <ul class="pop-up" v-show="popUp || matches==[]">
      <li v-for="(user,index) in matches" 
          :key="user['name']"
          @click="itemClicked(index)"
          :class="{'selected': (selected == index)}"
          >
          {{user.name}}
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  props: ['chatInput', 'users', ],
  data() {
    return {
      popUp: false,
      selected: 0,
      selectedItems: null,
      // itemHeight: 39,
    }
  },
  watch:{
  chatInput() {
      if(this.chatInput.match('@')){
         this.popUp = true 
        }else{
              this.popUp = false
        } 
      if(this.chatInput.match('@ ')){
              this.popUp = false
      }
    }
  },
  computed:{
    matches(){
      if(this.chatInput == ''){
        return []
      }
      if(this.popUp){
        //Filter list after @ values         
       return this.users.filter(item =>
          item['name'].toLowerCase().includes(this.chatInput.split("@")[1].toLowerCase())
       )
      }
      return this.chatInput
    },
    // popUp(){
    //   return this.chatInput.endsWith("@")
    // }
  },
  methods:{
    itemClicked(i){
      this.selected = i 
      this.selectItem()
    },
    selectItem(){
      let replaceInput = this.matches[this.selected]
      this.$emit('itemSelected', replaceInput)
      //focus back to input
      this.$refs.input.focus()
    },
    up() {
      if (this.selected == 0) {
        return
      }
      this.selected -= 1
      // this.scrollToItem();
    },
    down() {
      if (this.selected >= this.matches.length - 1) {
        return
      }
      this.selected += 1
      // this.scrollToItem();
    },
    // scrollToItem() {
    //   this.$refs.optionsList.scrollTop = this.selected * this.itemHeight
    // },
  }
}
</script>

<style scoped>
.x-input {
  position: relative;
  border: 1px solid darkgrey;
  display: inline-block;
  max-width: 500px;
  width: 100%;
}
.x-input input {
  width: 100%;
  box-sizing: border-box;
  border: none;
  bottom: 0;
  left: 0;
  margin: 0;
  outline: none;
  padding: 0 8px;
  height: 22px;
}
.x-input button {
  position: absolute;
  right: 0;
  top: 0;
}
.pop-up {
  list-style: none;
  position: absolute;
  border: 1px solid black;
  padding: 0;
  left: 0;
  right: 0;
  width: 74%;
  /* bottom: 18px; */
  margin: 0 auto;
  background-color: #fff;
}
.pop-up li.selected {
  background: #58bd4c;
  color: #fff;
  font-weight: 600;
}
</style>
