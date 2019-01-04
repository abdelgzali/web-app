nav<template>
  <div id="app">
    <img id="logo" alt="Vue logo" src="../public/logo.png">
    <div id="nav">
      <ul>
        <li><router-link to="/">Home</router-link></li>
        <li><router-link to="/about">About</router-link></li>
      </ul>
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
  width: 240px;
  height: auto;
}

#nav {
  position: relative;
  width: 100%;
  height: 50px;
  line-height: 50px;
  text-align: center;
  background: $secondary;
  margin-bottom: 20px;
}

/* NAVIGATION*/

#nav ul {
  list-style: none;
  display: flex;
  flex-flow: row-wrap;
  justify-content: center;
  text-align: center;
  margin: auto;
  padding: 0;
}

#nav li {
  display: inline-block;
  padding: 0 30px;
  color: #fff;
}

#nav li:hover {
  background: $primary;
  cursor: pointer;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
}

#nav a:-webkit-any-link {
  color: #fff;
  text-decoration: none;
}
</style>
