<template>
  <div id="app">
    <router-link to="/">home</router-link>  |
    <router-link to="/about">demoPage</router-link>  |
    <router-link to="/element">element</router-link>  |
    <router-link to="/miniateTrabsfer">miniateTrabsfer</router-link> |
    <router-link to="/optionSetting">optionSetting</router-link> |
    <router-link to="/erpTable">erpTable</router-link> | 
    <router-link to="/editCell">editCell</router-link> | 
    <div class="toTop" style="width:50px;height:50px;border-radius:50%;border:1px solid black;position:fixed;bottom:100px;right:20px;color:red;display:flex;justify-content:center;align-items:center;cursor:pointer" @click="toTop">回顶</div>
    <!-- miniateTrabsfer  optionSetting -->
    <router-view/>
  </div>
</template>
<script>
  export default {
    methods:{
      toTop() {
        if (document.documentElement) {
          document.documentElement.scrollTop = 0;
        } else if (document.body) {
          document.body.scrollTop = 0;
        }
      },
    }
  }
</script>

<style lang="scss" scoped>
  #app{
    // display:flex;
    // justify-content: center;
    // align-items: center;
  }
</style>
