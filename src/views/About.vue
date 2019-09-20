<template>
  <div class="about">
    <div id="nav" class="parent" @click="clickParent">
       <router-link to="/about">About</router-link>
      <div class="brother" @click="clickBrother"><span @click.stop="bson" class="bson">son</span></div>
      <div class="brother" @click="clickBrother" >b2</div>
      <div class="brother" @click="clickBrother" >b3</div>
      <div class="brother"  >b4</div>
      <!--  @click.stop="clickMySelf" -->
      <div class="mySelf"  @keyup.enter="like" tabindex="3" @focus="focusFn" @blur="blurFn">me</div>
      <input v-focus @click="showLayerFn" />
      <div class="layer" v-if="showLayer">
        <button v-focus @keydown.enter="clickFn">点击</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data:function(){
    return {
      showLayer:false,
    }
  },
  directives:{
    focus:{
      inserted(el){
        el.focus()
      }
    }
  },
  methods:{
    clickFn(){
      alert("enter")
    },
    showLayerFn () {
      this.showLayer = true
    },
    clickParent () {
      alert('parent')
    },
    clickBrother () {
      alert('brother')
    },
    clickMySelf () {
      alert('mySelf')
    } ,
    bson(){
      alert('bson')
    },
    like () {
      alert('like')
    },
    focusFn () {
      console.log('focus')
    },
    blurFn () {
      console.log('blur')
    }
  }
}
</script>>
<style>
.layer{
  position:fixed;
  top:0;
  right:0;
  bottom:0;
  left:0;
  background:rgb(0,0,0,0.5);
}
.parent{
  width:600px;
  height:600px;
  margin:0  auto;
  border:1px solid red;
  padding:20px;
  box-sizing: border-box;
  display:flex;
  flex-wrap:wrap;
}
.brother{
  width:100px;
  height:100px;
  border:1px solid green;
  margin-right:10px;
  display:flex;
  justify-content: center;
  align-items: center;
}
.mySelf{
  width:200px;
  height:200px;
  border:1px solid black;
}
.bson{
  display:inline-block;
  width:20px;
  height:20px;
  border:1px solid blue;
}
</style>

