<template lang="html">
  <div class="suggestions-container" v-if="suggestions.length > 0">

    <h3 id="suggestions-title">YOUR SUGGESTIONS</h3>

    <div class="filter-btns">
      <i class="material-icons">filter_list</i>
      <button
        type="button"
        name="filter-btn"
        v-for="(btn, index) in sortButtons"
        @click="toggleSort(index) || sortBy(btn.category)"
        :class="{active: btn.isActive}" >{{btn.category}}
      </button>
    </div>

    <div class="card" v-for="(suggested, index) in suggestions">
      <div class="name-tag">
        {{nameTag(suggested.NAME)}}
      </div>
      <div id="profile">
        <div id="profile-pic">
          <img v-bind:src="setImg(suggested.NAME)" alt="head-shot" id="head-shot">
        </div>
        <div id="profile-text">
          <div class="col" id="col-left">
            <div class="col-txt-left">
              <h4 id="value">VALUE</h4>
            </div>
            <div class="col-txt-left">
              <h4>Position</h4>
            </div>
            <div class="col-txt-left">
              <h4>Team</h4>
            </div>
          </div>
          <div class="col" id="col-right">
            <div class="col-txt-right">
              <h4 id="value">{{suggested.VALUE.toFixed(2)}}</h4>
            </div>
            <div class="col-txt-right">
              <h4>{{suggested.POS}}</h4>
            </div>
            <div class="col-txt-right">
              <h4>{{suggested.TEAM}}</h4>
            </div>
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
      // mutate playersList, don't mutate suggestions
      playersList: [],
      sortButtons: [
        {
          category: 'VALUE',
          isActive: false
        },
        {
          category: 'FG%',
          isActive: false
        },
        {
          category: 'FT%',
          isActive: false
        },
        {
          category: 'PTS',
          isActive: false
        },
        {
          category: '3P',
          isActive: false
        },
        {
          category: 'AST',
          isActive: false
        },
        {
          category: 'REB',
          isActive: false
        },
        {
          category: 'STL',
          isActive: false
        },
        {
          category: 'BLK',
          isActive: false
        },
        {
          category: 'TO',
          isActive: false
        }
      ]
    };
  },
  computed: {

    // computes list of suggestions for playercards, updates when playerList mutates
    suggestions: {
      get: function() {
        // prevent updating from parent prop when playersList isn't empty
        if(this.playersList.length == 0) {
          this.playersList = this.players;
        } else if (this.playersList.length > 0 && this.playersList[0] != this.players[0]) {
          this.playersList = this.players;
        }
        return this.playersList.slice(1,11);
      },
      set: function(newValue) {
        this.playersList = newValue;
        console.log(this.playersList);
      }
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
    // dynamic url's for profile pic based on name
    setImg(name) {
      let nameArray = name.split(/[\s.-]+/);
      let firstName = nameArray[0];
      let lastName = nameArray[1];
      if (nameArray.length > 2) {
        let suffix = nameArray[2];
        console.log("https://nba-players.herokuapp.com/players/" + lastName + "_" + suffix + "/" + firstName);
        return this.source = "https://nba-players.herokuapp.com/players/" + lastName + "_" + suffix + "/" + firstName
      }
      return this.source = "https://nba-players.herokuapp.com/players/" + lastName + "/" + firstName
    },

    nameTag(name) {
      if (name == this.searchedPlayer.NAME) {
        return name
      } else {
        return name + " for " + this.searchedPlayer.NAME;
      }
    },

    // re-sorts data from players prop (parent) and updates playersList
    sortBy(category) {
      let sortedList = this.players.slice(1,11);
      sortedList.sort((firstPlayer, secondPlayer) => {
        if (firstPlayer[category] < secondPlayer[category] ) {
          return 1
        } else if (firstPlayer[category] > secondPlayer[category]) {
          return -1
        } else {
          return 0
        }
      })
      sortedList.unshift(this.players[0]);
      this.playersList = sortedList;
    },

    toggleSort(index) {
      this.sortButtons.forEach((button) => {
        button.isActive = false
      });
      this.sortButtons[index].isActive = true;
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
}

button {
  background: none;
}

.suggestions-container {
  background-color: #f5f7f9;
  margin: 0 10vw;
  width: calc(100% - 20vw);
  padding: 20px 0;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  border-radius: 8px;

  /* fade animation */
  -webkit-animation: fadein 2s; /* Safari, Chrome and Opera > 12.1 */
  -moz-animation: fadein 2s; /* Firefox < 16 */
  -ms-animation: fadein 2s; /* Internet Explorer */
  -o-animation: fadein 2s; /* Opera < 12.1 */
  animation: fadein 2s;
}

#suggestions-title {
  margin: 40px auto;
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
  width: calc(100% - 20px);
  height: auto;
  padding: 5vh 10px;
}
#profile-pic {
  width: 60%;
  padding: 0 5%;
}
#head-shot {
  width: 100%;
}
#profile-text {
  @include flex-row;
}
#col-left {
  @include flex-col;
  text-align: left;
  justify-content: space-around;
  padding: 0 10px;
}
.col-txt-left {
  align-content: center;
}
#col-right {
  @include flex-col;
  text-align: right;
  justify-content: space-around;
  padding: 0 10px;
}
#value {
  color: $primary;
}

#stats {
  padding-top: 12px;
}

/* FILTERS */
.material-icons {
  margin: auto 5px;
}
.filter-btns {
  @include flex-row;
  justify-content: space-between;
  box-shadow: 0 1px 0 rgba(12,13,14,0.1), 0 1px 6px rgba(59,64,69,0.1);
  overflow-x: scroll;
}
.filter-btns button {
  border: none;
  cursor: pointer;
  height: 40px;
  width: 100%;
}

.active {
  background-color: $secondary;
  color: #fff;
  font-weight: bold;
  background-position: center;
  transition: background 0.5s;
}


/* STATS */
#stats {
  @include flex-row;
  overflow: scroll;
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
  background-color: rgba(22, 160, 133,1.0);
  color: #fff;
}

@media only screen and (max-width: 48em) {
  .card {
    width: calc(100% - 30px);
    font-size: 12px;
  }
  #profile-pic {
    width: 50%;
  }
}

@media only screen and (max-width: 30em) {
  .filter-btns {
    display: flex;
    flex-flow: row wrap;
    justify-content: space-between;
  }
  .filter-btns button {
    width: 52px;
  }
}

/* animation keyframes */
@keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Firefox < 16 */
@-moz-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Safari, Chrome and Opera > 12.1 */
@-webkit-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Internet Explorer */
@-ms-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Opera < 12.1 */
@-o-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

</style>
