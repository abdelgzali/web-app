<template>
  <div id="app">
    <div id="nav">
      <Nav/>
      <router-link :to="{ name: 'home' }">
        <img id="logo" alt="Vue logo" src="../public/logo2.png">
      </router-link>
    </div>
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
#logo {
  position: absolute;
  height: 75px;
  width: auto;
  left: 20px;
  top: 20px;
}

@media only screen and (max-width: 48em) {
  #logo {
    left: auto;
    right: 20px;
    height: 50px;
  }
  #menu {
    position: absolute;
    top: 20px;
    left: 20px;
    color: #fff;
  }
}

</style>
