<template>
  <div data-reactroot="" class="Root">
    <div
      class="Root-unsplashBackgroundImage"
      :style="{ backgroundImage: 'url(' + bgurl + ')' }"
    ></div>
    <div class="Root-keyboardLayout">
      <div class="KeyboardRow">
        <div style="position: relative" v-for="(item,i) in list" :key="item.key" v-if="i<10" v-on:mouseover="hoverKey(item.key,i)">
          <div draggable="true">
            <div
              :class="['Key isFocused',item.hoverClass]" :data-key="item.key"
            >{{item.key}}
            </div>
          </div>
        </div>
        
      </div>
      <div class="KeyboardRow">
        <div style="position: relative" v-for="(item,i) in list" :key="item.key" v-if="i>=10&&i<19" v-on:mouseover="hoverKey(item.key,i)">
          <div draggable="true">
            <div
               :class="['Key isFocused',item.hoverClass]" :data-key="item.key"
            >{{item.key}}</div>
          </div>
        </div>
      </div>
      <div class="KeyboardRow">
        <div style="position: relative" v-for="(item,i) in list" :key="item.key" v-if="i>=19" v-on:mouseover="hoverKey(item.key,i)">
          <div draggable="true">
            <div
               :class="['Key isFocused',item.hoverClass]" :data-key="item.key"
            >{{item.key}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data: function () {
    return {
      bgurl:
        "https://images.unsplash.com/photo-1456428199391-a3b1cb5e93ab?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMjEzN3wwfDF8cmFuZG9tfHx8fHx8fHx8MTYxNzg3MjY1Mg&ixlib=rb-1.2.1&q=80&w=1080",
        list:[{
          key:'q',
          url:'',
          icon:''
        },{
          key:'w',
          url:'',
          icon:''
        },{
          key:'e',
          url:'',
          icon:''
        },{
          key:'r',
          url:'',
          icon:''
        },{
          key:'t',
          url:'',
          icon:''
        },{
          key:'y',
          url:'',
          icon:''
        },{
          key:'u',
          url:'',
          icon:''
        },{
          key:'i',
          url:'',
          icon:''
        },{
          key:'o',
          url:'',
          icon:''
        },{
          key:'p',
          url:'',
          icon:''
        },{
          key:'a',
          url:'',
          icon:''
        },{
          key:'s',
          url:'',
          icon:''
        },{
          key:'d',
          url:'',
          icon:''
        },{
          key:'f',
          url:'',
          icon:''
        },{
          key:'g',
          url:'',
          icon:''
        },{
          key:'h',
          url:'',
          icon:''
        },{
          key:'j',
          url:'',
          icon:''
        },{
          key:'k',
          url:'',
          icon:''
        },{
          key:'l',
          url:'',
          icon:''
        },{
          key:'z',
          url:'',
          icon:''
        },{
          key:'x',
          url:'',
          icon:''
        },{
          key:'c',
          url:'',
          icon:''
        },{
          key:'v',
          url:'',
          icon:''
        },{
          key:'b',
          url:'',
          icon:''
        },{
          key:'n',
          url:'',
          icon:''
        },{
          key:'m',
          url:'',
          icon:''
        }],
    };
  },
  methods:{
    hoverKey:function (key,i) {
      console.log(key,i)
      // this.$set(this.list[i], `hoverClass`, "isFocused")
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Root-unsplashBackgroundImage {
  height: 100vh;
  width: 100vw;
  background-size: cover;
  background-position: 50%;
  background-repeat: no-repeat;
  position: absolute;
  z-index: 0;
  transform: scale(1);
  transition: all 0.25s;
  opacity: 1;
  animation-name: fadeInOpacity;
  animation-timing-function: ease-in;
  animation-duration: 0.25s;
}
.Root-keyboardLayout {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 100vh;
  position: relative;
  z-index: 2;
}
.KeyboardRow {
  display: flex;
  margin: 12px 0;
}

.Key {
  font-family: Ropa Sans, sans-serif;
  text-transform: uppercase;
  font-size: 35px;
  line-height: 52px;
  width: 100px;
  height: 100px;
  border-radius: 15px;
  color: #fff;
  background-color: hsla(0, 0%, 100%, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 8px;
  margin-top: 8px;
  position: relative;
  box-shadow: 0 0 0 0 rgba(0, 0, 0, 0.85);
  border: 2px solid #e5e5e5;
}
.Key.isDraggedOver {
  border: 2px solid #396ffe;
}
.Key.full {
  opacity: 1;
}
.Key.empty {
  opacity: 0.3;
}
.Key.empty.isDraggedOver {
  opacity: 1;
}
.Key:active {
  top: 2px;
}
.Key.launched,
.Key:active {
  border: 2px solid #396ffe;
  animation-name: launchKeyAnimation;
  animation-timing-function: ease-in;
  animation-duration: 0.25s;
}
@keyframes launchKeyAnimation {
  0% {
    border-color: #e5e5e5;
    bottom: 0;
  }
  50% {
    border-color: #396ffe;
    top: 2px;
  }
  to {
    border-color: #fff;
    top: 0;
  }
}
.Key.isFocused:hover {
  opacity: 1;
  margin-top: 5px;
  box-shadow: 0 4px 4px 4px rgba(0, 0, 0, 0.25);
  animation-name: hoverUp;
  animation-timing-function: ease-out;
  animation-duration: 0.15s;
}
@keyframes hoverUp {
  0% {
    margin-top: 8px;
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0.85);
  }
  to {
    margin-top: 5px;
    box-shadow: 0 4px 4px 4px rgba(0, 0, 0, 0.25);
  }
}
.Key.empty:hover .Key {
  opacity: 1;
}
.Key.empty:hover .Key-addButton,
.Key.empty:hover .Key-letter {
  opacity: 0.3;
}
.Key .EmojiExists {
  opacity: 1;
}
.Key.full:hover .EmojiExists {
  opacity: 0.5;
}
.Key.full:hover .EmojiExists:hover {
  opacity: 1;
}
.Key.full:hover .EmojiDoesNotExist {
  opacity: 0.5;
}
.Key.full:hover .EmojiDoesNotExist:hover {
  opacity: 1;
}
.Key.full:hover .Key-deleteButton {
  opacity: 0.5;
}
.Key.full:hover .Key-deleteButton:hover {
  opacity: 1;
}
.Key-letter {
  top: 0;
  left: 0;
}
.Key-letter,
.Key-letter.hover {
  position: relative;
  transition: all 0.2s;
}
.Key-letter.hover {
  top: -30px;
  left: 30px;
  font-size: 20px;
  opacity: 1;
}
.Key-addButton {
  position: absolute;
  visibility: hidden;
}
.Key-addButton.hover {
  visibility: visible;
  font-size: 55px;
  margin-left: -7px;
}
.Key-deleteButton {
  position: absolute;
  right: 5px;
  bottom: 8px;
  font-size: 14px;
  line-height: 14px;
  width: 24px;
  border-radius: 15px;
  color: #fff;
  cursor: pointer;
  opacity: 0;
}
.Key-deleteButton.hover {
  opacity: 1;
}
.Key-addEmojiButton {
  position: absolute;
  right: 8px;
  top: 8px;
  font-size: 24px;
  line-height: 24px;
  opacity: 0;
  cursor: pointer;
}
.Key-favicon {
  position: absolute;
  bottom: -10px;
}
.Key-addUrl::-webkit-input-placeholder {
  color: #fff;
  opacity: 0.5;
  text-align: center;
}
.Key-addUrl {
  font-family: Ropa Sans;
  background: linear-gradient(20deg, #396ffe, #33d0d8);
  color: #fff;
  text-align: left;
  border-radius: 5px;
  width: 150px;
  position: absolute;
  z-index: 10;
  justify-content: center;
  align-items: center;
  top: 140px;
  left: -15px;
  line-height: normal;
  border: none;
  word-wrap: break-word;
  padding: 8px;
  font-size: 14px;
}
input:focus {
  outline: none;
}
.emoji-mart-emoji span {
  height: 24px !important;
  width: 24px !important;
}
@media (min-width: 1101px) and (max-width: 1325px) {
  .Key {
    height: 85px;
    width: 85px;
    font-size: 30px;
  }
  .Key-addButton.hover {
    font-size: 47px;
    margin-left: -6px;
  }
  .emoji-mart-emoji span {
    height: 21px !important;
    width: 21px !important;
  }
  .Key-addUrl {
    top: 119px;
    left: -30px;
  }
  .Key-deleteButton {
    font-size: 12px;
  }
  .Key-letter.hover {
    position: relative;
    top: -26px;
    left: 26px;
    transition: all 0.2s;
    font-size: 17px;
  }
}
@media (max-width: 1100px) {
  .Key {
    height: 75px;
    width: 75px;
    font-size: 26px;
  }
  .Key-deleteButton {
    font-size: 10px;
  }
  .Key-addButton.hover {
    font-size: 41px;
    margin-left: -5px;
  }
  .emoji-mart-emoji span {
    height: 18px !important;
    width: 18px !important;
  }
  .Key-addUrl {
    top: 105px;
    left: -37px;
  }
  .Key-letter.hover {
    position: relative;
    top: -22px;
    left: 22px;
    transition: all 0.2s;
    font-size: 15px;
  }
}
</style>
