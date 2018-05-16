<template>
  <div id="app">
    <div class="main">
      <router-view></router-view>
    </div> 
    
  </div> 
</template>

<script>
export default {
  name: 'app',
  
};
</script>

<style>
#app {
  font-family: 'Microsoft YaHei','Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height:100% ;
}

.main{
  float:left;
  width:100%;
  background-color: #EFF2F7;
  height:100%;
  overflow: auto;

}
</style>
