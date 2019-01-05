nav<template>
  <div id="app">
    <div id="nav">
      <ul>
        <li><router-link to="/"></router-link></li>
        <li><router-link to="/about"></router-link></li>
      </ul>
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
}
#logo {
  height: 100px;
  width: auto;
}

#nav {
  position: relative;
  width: 100%;
  height: auto;
  text-align: center;
  padding: 10px 0;
  background: #000;
}

/* NAVIGATION*/

#nav ul {
  list-style: none;
  display: flex;
  flex-flow: row-wrap;
  justify-content: flex-end;
  text-align: center;
  margin: auto;
  overflow: visible;
  padding: 20px 0 10px 0;

}

#nav li {
  display: inline-block;
  padding: 10px 20px;
  color: #fff;
}

#nav li:hover {
  cursor: pointer;
}

#nav a:-webkit-any-link {
  padding: 0 20px;
  color: #fff;
  text-decoration: none;
}

#nav a:-webkit-any-link:hover {
  text-decoration: underline;
}

@media only screen and (max-width: 600px) {

}

</style>
