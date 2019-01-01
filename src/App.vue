<template>
  <div id="app">
 <leftnav></leftnav>
   <div class="ming">
   <router-view></router-view>
   </div>
  </div>
</template>

<script>
import leftnav from '@/components/common/leftnav'
export default {
  name: 'App',
  components: {
    leftnav
  }
}
</script>

<style>
#app {
  font-family:'Microsoft yaHe' 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
 
}
.ming{
  width: 95%;
  float: left;
  background: #eff2f7;
  height: 100%;
  overflow: hidden;
}

</style>
