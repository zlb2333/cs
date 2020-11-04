<template>
  <div id="app">
    <left-nav/>
    <div class="main"><router-view/></div>
   
  </div>
</template>

<script>

import leftNav from './components/common/leftNav'

export default {
 
  name: 'App',
  components:{
    leftNav
  }
}
</script>

<style>
.main{
float: left;
width: 95%;
background-color: #eff2f7;
height: 100%;
overflow: hidden;
text-align: center;
}

</style>
// 6666666666666666