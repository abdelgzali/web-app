nav<template>
  <div id="app">
    <div id="nav">
      <img id="logo" alt="Vue logo" src="../public/logo2.png">
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">

$primary: #F21A13;
$secondary: #1D428A;

#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-bottom: 30px;

  /* gradient */
  background: #0F2027;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to left, #2C5364, #203A43, #0F2027);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to left, #2c3e50, #203A43, #0F2027); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

}
#logo {
  position: absolute;
  height: 75px;
  width: auto;
  left: 20px;
  top: 20px;
}

@media only screen and (max-width: 600px) {
  #logo {
    height: 50px;
  }
}

</style>
