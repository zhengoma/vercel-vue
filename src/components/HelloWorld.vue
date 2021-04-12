<template>
  <div data-reactroot="" class="Root">
    <div
      class="Root-unsplashBackgroundImage"
      :style="{ backgroundImage: 'url(' + bgurl + ')' }"
    >
        <div class="overlay"></div>
      <p class="note">{{bgnote}}<br><small>{{bgcontent}}</small></p>
    </div>
    <div class="Root-keyboardLayout">
      <div class="KeyboardRow"  v-for="(row,r) in rowArr" :key="r">
        <div style="position: relative" v-for="(item,i) in list" :key="item.key" v-if="i>=row.min && i<row.max" @click="gotoEvt(item.url)">
          <div draggable="true">
            <div :class="['Key isFocused']" :data-key="item.key" :id="i">
              <span>{{item.key}}</span>
              <div v-if="item.url" class="delete" @click="deleteEvt(item.key,i)"></div>
              <div v-else class="add" @click="addEvt(item.key,i)"></div>
              <input v-if="item.edit" v-focus placeholder="https://……" class="edit" @keyup.enter="onSubmit" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import fetchJsonp from 'fetch-jsonp';
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data: function () {
    return {
      hash: '',
      rowArr:[{min:0,max:10},{min:10,max:19},{min:19,max:30}],
      bgurl: '',
      bgnote: '',
      bgcontent: '',
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
  created(){
    let hash = location.hash||'';
    console.log(hash)
    if(hash.replace('#','')===''){
      hash = localStorage.getItem('myzTab')||'';
      if(hash.replace('#','')===''){
          hash = String(Date.now());
          localStorage.setItem('myzTab',hash)
      }
      location.hash = hash;
    }
    this.hash = hash.replace('#','');

    let bgurl = localStorage.getItem('myzBg');
    if(bgurl){
      this.bgurl = bgurl;
    }

    let listStr = localStorage.getItem(this.hash);
    if(listStr){
      this.list = JSON.parse(listStr);
      console.log(this.list);
    }
    
    this.getPaper();
    this.getApi(this.hash)
    
  },
  methods:{
    gotoEvt:function (url) {
      if(url){
        window.open(url)
      }
    },
    addEvt:function (key,i) {
      for (let index = 0; index < this.list.length; index++) {
        if(i===index){
          this.$set(this.list[index], `edit`, true) 
        }else{
          this.$set(this.list[index], `edit`, false) 
        }
      }
    },
    onSubmit:function (e) {
      // console.log(e)
      let value = e.target.value;
      let i = e.target.parentNode.id;
      let oldVal = this.list[i];
      oldVal.url=value;
      oldVal.edit=false;
      this.$set(this.list,i,oldVal)

      localStorage.setItem(this.hash,JSON.stringify(this.list));

      this.setApi(this.hash, this.list[i]['key'], this.list[i]['url'])
    },
    deleteEvt:function (key,i) {
      this.$set(this.list[i], `url`, '')
    },
    setApi:function(hash,key,url){
      console.log(hash,key,url)
      fetch(`/backend/mango/?cmd=navset&hash=${hash}&key=${key}&url=${url}`, {
        method: 'GET',
        redirect: 'follow'
      })
      .then(response => response.json())
      .then(result => console.log(result))
      .catch(error => console.log('error', error));
    },
    getApi:function(hash){
      console.log(hash)
      fetch(`/backend/mango/?cmd=navget&hash=${hash}`, {
        method: 'GET',
        redirect: 'follow'
      })
      .then(response => response.json())
      .then(result =>{

        result.data && result.data.forEach(element => {
          for (let index = 0; index < this.list.length; index++) {
            const item = this.list[index];
            if(item.key===element.key){
                this.$set(this.list[index], `url`, element.url)
            }
          }
        });
      })
      .catch(error => console.log('error', error));
    },
    getPaper: function(){
      fetchJsonp("/backend/paper/",{
        jsonpCallback:"callback"
      }).then(res=>res.json()).then(res=>{
         this.bgnote = res.note;
         this.bgcontent = res.content;
         this.bgurl = res.picture4;
         localStorage.setItem('myzBg',this.bgurl)
      });
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.overlay{
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 10;
		background: rgba(0, 0, 0, 0.5);
	}
.note{
		color: #fff;
		width: 90%;
		font-weight: 300;
		font-size: 30px;
		position: absolute;
		top: 20%;
		z-index: 20;
		font-family: 微软雅黑;	
		padding:0 5%;		
    line-height: 40px;
	}
.edit{
  position: absolute;
    right: -26px;
    bottom: -32px;
    width: 160%;
    height: 22px;
    background: linear-gradient(20deg, #396ffe, #33d0d8);
    border: 0;
    border-radius: 6px;
    color: #fff;
    
}
.edit::-webkit-input-placeholder {
  color: #fff;
  opacity: 0.5;
  text-align: left;
  padding-left: 5px;
}
.add{
    display:none;
    position: absolute;
    right: 4px;
    bottom: 4px;
    width: 25%;
    height: 25%;
    background-size: cover;
    background-image: url(http://image-change.test.upcdn.net/1618199491324/add.png);
}
.delete{
  display:none;
    position: absolute;
    right: 4px;
    top: 4px;
    width: 25%;
    height: 25%;
    background-size: cover; 
    background-image: url(http://image-change.test.upcdn.net/1618199491324/trash.png);
}

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
  height: 60vh;
  top: 40vh;
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
.Key.isFocused:hover .add{
    display: block;
}
.Key.isFocused:hover .delete{
    display: block;
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
 
 
.Key-favicon {
  position: absolute;
  bottom: -10px;
}

input:focus {
  outline: none;
}
 
@media (min-width: 1101px) and (max-width: 1325px) {
  .Key {
    height: 85px;
    width: 85px;
    font-size: 30px;
  }
   
}
@media (max-width: 1100px) {
  .Key {
    height: 75px;
    width: 75px;
    font-size: 26px;
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
