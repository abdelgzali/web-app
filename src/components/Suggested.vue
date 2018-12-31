<template lang="html">
  <div class="parent-container">
    <div class="card" v-for="(suggested, index) in suggestions">
      <div id="profile">
        <div id="profile-pic">
          <img v-bind:src="setImg(suggested.NAME)" alt="head-shot" id="head-shot">
          <h3>{{suggested.NAME}}</h3>
        </div>
        <div id="profile-text">
          <h4>2018 Season stats (per game):</h4>
          <p id="value">VALUE: {{suggested.VALUE.toFixed(2)}}</p>
        </div>
      </div>

      <div id="stats">
        <div class="stat-box">
          <h4>FG%</h4>
          <p>{{(suggested["FG%"]*100).toFixed(1) }}</p>
        </div>
        <div class="stat-box">
          <h4>FT%</h4>
          <p>{{(suggested["FT%"]* 100).toFixed(1)}}</p>
        </div>
        <div class="stat-box">
          <h4>PPG</h4>
          <p>{{suggested.PTS.toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>3P</h4>
          <p>{{suggested["3P"].toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>AST</h4>
          <p>{{suggested.AST.toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>REB</h4>
          <p>{{suggested.REB.toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>STL</h4>
          <p>{{suggested.STL.toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>BLK</h4>
          <p>{{suggested.BLK.toFixed(2)}}</p>
        </div>
        <div class="stat-box">
          <h4>TO</h4>
          <p>{{suggested.TO.toFixed(2)}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['players','name'],
  data() {
    return {
    };
  },
  computed: {
    suggestions() {
      console.log(this.players + "...players logged");
      return this.players;
    }
  },
  methods: {
    setImg(name) {
      console.log(name);
      let firstName = name.split(' ').slice(0, -1).join(' ');
      let lastName = name.split(' ').slice(-1).join(' ');
      return this.source = "https://nba-players.herokuapp.com/players/" + lastName + "/" + firstName
    },
  }
}
</script>

<style lang="scss">

$primary: #F21A13;
$secondary: #1D428A;

@mixin flex-row {
  display: flex;
  flex-flow: row nowrap;
}

@mixin flex-col {
  display: flex;
  flex-flow: column wrap;
  align-items: stretch;
}

.card {
  @include flex-col;
  background: rgb(255, 255, 255);
  padding: 20px 40px;
  margin: 20px auto;
  width: 80%;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

#stats {
  @include flex-row;
  overflow: scroll;
}

.stat-box {
  background-color: $secondary;
}

.stat-box h4 {
  color: #fff;
  margin: 0;
  padding: 2px 6px;
}

.stat-box p {
  background-color: #fff;
  padding: 16px 0;
  margin: 0;
}
</style>
