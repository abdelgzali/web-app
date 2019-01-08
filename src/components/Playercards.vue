<template lang="html">
  <div class="parent-container">
    <div class="filter-by">
      <p>Filter by:</p>
      <button type="button" name="filter-btn" @click="sortBy('FG%')">FG%</button>
      <button type="button" name="filter-btn" @click="sortBy('FT%')">FT%</button>
      <button type="button" name="filter-btn" @click="sortBy('PTS')">PTS</button>
      <button type="button" name="filter-btn" @click="sortBy('3P')">3P</button>
      <button type="button" name="filter-btn" @click="sortBy('AST')">AST</button>
      <button type="button" name="filter-btn" @click="sortBy('REB')">REB</button>
      <button type="button" name="filter-btn" @click="sortBy('STL')">STL</button>
      <button type="button" name="filter-btn" @click="sortBy('BLK')">BLK</button>
      <button type="button" name="filter-btn" @click="sortBy('TO')">TO</button>
    </div>
    <div class="card" v-for="(suggested, index) in suggestions" v-model="suggestions">
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
          <p :class="{
            'pos': difference[index].VALUE > 0,
            'neg': difference[index].VALUE < 0
            }">{{difference[index].VALUE}}</p>
        </div>
        <div class="stat-box">
          <h4>MPG</h4>
          <p>{{suggested.MINS}}</p>
          <p>{{searchedPlayer.MINS}}</p>
          <p :class="{
            'pos': difference[index].MINS > 0,
            'neg': difference[index].MINS < 0
            }">{{difference[index].MINS}}</p>
        </div>
        <div class="stat-box">
          <h4>FG%</h4>
          <p>{{(suggested["FG%"]*100).toFixed(1)}}</p>
          <p>{{(searchedPlayer["FG%"]*100).toFixed(1)}}</p>
          <p :class="{
            'pos': difference[index].FG > 0,
            'neg': difference[index].FG < 0
            }">{{difference[index].FG}}</p>
        </div>
        <div class="stat-box">
          <h4>FT%</h4>
          <p>{{(suggested["FT%"]* 100).toFixed(1)}}</p>
          <p>{{(searchedPlayer["FT%"]* 100).toFixed(1)}}</p>
          <p :class="{
            'pos': difference[index].FT > 0,
            'neg': difference[index].FT < 0
            }">{{difference[index].FT}}</p>
        </div>
        <div class="stat-box">
          <h4>PPG</h4>
          <p>{{suggested.PTS.toFixed(1)}}</p>
          <p>{{searchedPlayer.PTS.toFixed(1)}}</p>
          <p :class="{
            'pos': difference[index].PTS > 0,
            'neg': difference[index].PTS < 0
            }">{{difference[index].PTS}}</p>
        </div>
        <div class="stat-box">
          <h4>3P</h4>
          <p>{{suggested["3P"].toFixed(2)}}</p>
          <p>{{searchedPlayer["3P"].toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].THREES > 0,
            'neg': difference[index].THREES < 0
            }">{{difference[index].THREES}}</p>
        </div>
        <div class="stat-box">
          <h4>AST</h4>
          <p>{{suggested.AST.toFixed(2)}}</p>
          <p>{{searchedPlayer.AST.toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].AST > 0,
            'neg': difference[index].AST < 0
            }">{{difference[index].AST}}</p>
        </div>
        <div class="stat-box">
          <h4>REB</h4>
          <p>{{suggested.REB.toFixed(2)}}</p>
          <p>{{searchedPlayer.REB.toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].REB > 0,
            'neg': difference[index].REB < 0
            }">{{difference[index].REB}}</p>
        </div>
        <div class="stat-box">
          <h4>STL</h4>
          <p>{{suggested.STL.toFixed(2)}}</p>
          <p>{{searchedPlayer.STL.toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].STL > 0,
            'neg': difference[index].STL < 0
            }">{{difference[index].STL}}</p>
        </div>
        <div class="stat-box">
          <h4>BLK</h4>
          <p>{{suggested.BLK.toFixed(2)}}</p>
          <p>{{searchedPlayer.BLK.toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].BLK > 0,
            'neg': difference[index].BLK < 0
            }">{{difference[index].BLK}}</p>
        </div>
        <div class="stat-box">
          <h4>TO</h4>
          <p>{{suggested.TO.toFixed(2)}}</p>
          <p>{{searchedPlayer.TO.toFixed(2)}}</p>
          <p :class="{
            'pos': difference[index].TO > 0,
            'neg': difference[index].TO < 0
            }">{{difference[index].TO}}</p>
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
      posValue: true,
      diff: 5,
    };
  },
  computed: {
    suggestions() {
      console.log(this.players + "...players logged");
      let finalMatchList = this.players.slice(1,11);
      return finalMatchList;
    },
    searchedPlayer() {
      return this.players[0];
    },
    // maps array of objects of +/- values for each suggested player
    difference() {
      return this.suggestions.map((suggested) => {
        let diffValues = {
          VALUE: (suggested.VALUE - this.searchedPlayer.VALUE).toFixed(3),
          MINS: (suggested.MINS - this.searchedPlayer.MINS).toFixed(2),
          FG: (suggested["FG%"]*100 - this.searchedPlayer["FG%"]*100).toFixed(1),
          FT: (suggested["FT%"]*100 - this.searchedPlayer["FT%"]*100).toFixed(1),
          PTS: (suggested.PTS - this.searchedPlayer.PTS).toFixed(1),
          THREES: (suggested["3P"] - this.searchedPlayer["3P"]).toFixed(2),
          AST: (suggested.AST - this.searchedPlayer.AST).toFixed(2),
          REB: (suggested.REB - this.searchedPlayer.REB).toFixed(2),
          STL: (suggested.STL - this.searchedPlayer.STL).toFixed(2),
          BLK: (suggested.BLK - this.searchedPlayer.BLK).toFixed(2),
          TO: (suggested.TO - this.searchedPlayer.TO).toFixed(2),

        }
        return diffValues;
      })
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
    sortBy(category) {
      let sortedList = this.players.slice(1,11);
      console.log("sorting...")
      sortedList.sort((firstPlayer, secondPlayer) => {
        if (firstPlayer[category] < secondPlayer[category] ) {
          console.log(firstPlayer[category]);
          return 1
        } else if (firstPlayer[category] > secondPlayer[category]) {
          return -1
        } else {
          return 0
        }
      })
      sortedList.unshift(this.players[0]);
      console.log(sortedList);
      this.players = sortedList;
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
