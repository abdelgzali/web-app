<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import Nav from '@/components/Nav'

 export default {
   components: {
     Nav
   }
 }
</script>

<style lang="scss">

$primary: #F21A13;
$secondary: #1D428A;

#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}

</style>
