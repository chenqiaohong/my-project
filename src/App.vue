<template>
  <div id="app">
    <leftNav></leftNav>
    <div class="main">
      <router-view></router-view>
    </div>
    
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav';
  export default{
    name: 'app',
    components:{
      leftNav
    }
  }
</script>

<style>

*, *::before, *::after {
  box-sizing: border-box;
}


h1 {
  text-align: center;
}
</style>
