<template lang="html">
  <div class="parent-container">
    <div class="card" v-for="(suggested, index) in suggestions">
      <div class="name-tag">
        {{nameTag(suggested.NAME)}}
      </div>
      <div id="profile">
        <div id="profile-pic">
          <img v-bind:src="setImg(suggested.NAME)" alt="head-shot" id="head-shot">
        </div>
        <div id="profile-text">
          <div class="row" id="value">
            <h4 id="value">VALUE</h4>
            <p>{{suggested.VALUE.toFixed(2)}}</p>
          </div>
          <div class="row">
            <h4>Position</h4>
            <p>{{suggested.POS}}</p>
          </div>
          <div class="row">
            <h4>Team</h4>
            <p>{{suggested.TEAM}}</p>
          </div>
        </div>
      </div>

      <h4>2018 Season stats (per game):</h4>
      <div id="stats">
        <div class="stat-box">
          <h4>PLAYER</h4>
          <p id="name-bold">{{suggested.NAME}}</p>
          <p id="name-bold">{{searchedPlayer.NAME}}</p>
          <p id="name-bold">+/-</p>
        </div>
        <div class="stat-box">
          <h4>VALUE</h4>
          <p>{{suggested.VALUE.toFixed(3)}}</p>
          <p>{{searchedPlayer.VALUE.toFixed(3)}}</p>
          <p :class="{}">{{ (searchedPlayer.VALUE - suggested.VALUE).toFixed(3)}}</p>
        </div>
        <div class="stat-box">
          <h4>MPG</h4>
          <p>{{suggested.MINS}}</p>
          <p>{{searchedPlayer.MINS}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.MINS, suggested.MINS)}}</p>
        </div>
        <div class="stat-box">
          <h4>FG%</h4>
          <p>{{(suggested["FG%"]*100).toFixed(1)}}</p>
          <p>{{(searchedPlayer["FG%"]*100).toFixed(1)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer["FG%"]*100, suggested["FG%"]*100)}}</p>
        </div>
        <div class="stat-box">
          <h4>FT%</h4>
          <p>{{(suggested["FT%"]* 100).toFixed(1)}}</p>
          <p>{{(searchedPlayer["FT%"]* 100).toFixed(1)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer["FT%"]*100, suggested["FT%"]*100)}}</p>
        </div>
        <div class="stat-box">
          <h4>PPG</h4>
          <p>{{suggested.PTS.toFixed(1)}}</p>
          <p>{{searchedPlayer.PTS.toFixed(1)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.PTS, suggested.PTS)}}</p>
        </div>
        <div class="stat-box">
          <h4>3P</h4>
          <p>{{suggested["3P"].toFixed(2)}}</p>
          <p>{{searchedPlayer["3P"].toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer["3P"], suggested["3P"])}}</p>
        </div>
        <div class="stat-box">
          <h4>AST</h4>
          <p>{{suggested.AST.toFixed(2)}}</p>
          <p>{{searchedPlayer.AST.toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.AST, suggested.AST)}}</p>
        </div>
        <div class="stat-box">
          <h4>REB</h4>
          <p>{{suggested.REB.toFixed(2)}}</p>
          <p>{{searchedPlayer.REB.toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.REB, suggested.REB)}}</p>
        </div>
        <div class="stat-box">
          <h4>STL</h4>
          <p>{{suggested.STL.toFixed(2)}}</p>
          <p>{{searchedPlayer.STL.toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.STL, suggested.STL)}}</p>
        </div>
        <div class="stat-box">
          <h4>BLK</h4>
          <p>{{suggested.BLK.toFixed(2)}}</p>
          <p>{{searchedPlayer.BLK.toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.BLK, suggested.BLK)}}</p>
        </div>
        <div class="stat-box">
          <h4>TO</h4>
          <p>{{suggested.TO.toFixed(2)}}</p>
          <p>{{searchedPlayer.TO.toFixed(2)}}</p>
          <p :class="{}">{{calcDiff(searchedPlayer.TO, suggested.TO)}}</p>
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
      //console.log(this.players + "...players logged");
      return this.players.slice(1,11);
    },
    searchedPlayer() {
      return this.players[0];
    },
  },
  methods: {
    setImg(name) {
      let firstName = name.split(' ').slice(0, -1).join(' ');
      let lastName = name.split(' ').slice(-1).join(' ');
      return this.source = "https://nba-players.herokuapp.com/players/" + lastName + "/" + firstName
    },
    nameTag(name) {
      if (name == this.searchedPlayer.NAME) {
        return name
      } else {
        return name + " for " + this.searchedPlayer.NAME;
      }
    },
    calcDiff(valOne,valTwo) {
      return (Number(valOne) - Number(valTwo)).toFixed(1)
    }
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
  margin: 20px auto;
  width: 80%;
  height: auto;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.name-tag {
  height: 30px;
  width: 100%;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #000;
  color: #fff;
  font-weight: bold;
}

#profile {
  @include flex-row;
  justify-content: space-around;
  width: 100%;
  height: auto;
  padding: 20px 0;
}
#profile-pic {
  width: 33%;
}
#head-shot {
  width: 100%;
  padding: 5px;
}

#profile-text {
  text-align: left;
  margin: auto 0;
  width: 50%;
}
#value {
  color: $primary;
}

#stats {
  @include flex-row;
  overflow: scroll;
}

.row {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #ecf0f1;

  h4 {
    color: #66737C;
    margin: auto 0;
  }
}

#stats {
  padding-top: 12px;
}

.stat-box {
  width: 100%;
  background-color: #fff;
  white-space: nowrap;
}

.stat-box h4 {
  background-color: $secondary;
  color: #fff;
  margin: 0;
  padding: 2px 6px;
}

#stats p {
  padding: 16px 5px;
  margin: 0;
}

.stat-box p, h4 {
  font-size: 14px;
  margin: auto;
}

#name-bold {
  font-weight: bold;
}

.neg {
  background-color: $primary;
  color: #fff;
}

.pos {
  background-color: #27ae60;
  color: #fff;
}

@media only screen and (max-width: 600px) {
  .card {
    width: 300px;
    font-size: 12px;
  }
  #profile-pic {
    width: 60%;
  }
  #profile-text {
    width: 40%;
    padding: 0 20px;
  }
}
</style>
