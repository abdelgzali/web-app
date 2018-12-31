<template lang="html">
  <div class="parent-container">
    <div class="search">
      <input class="search-bar" placeholder="search player" v-model="searchName" @input="findPlayer">
      <Autocomplete :items="playerNames" v-bind:tQuery="searchName" @selected="playerSelected"/>
    </div>
    <div class="player-card">
      <div id="profile">
        <div id="profile-pic">
          <img v-bind:src="source" alt="head-shot" id="head-shot">
          <h3>{{player}}</h3>
        </div>
        <div id="profile-text">
          <h4>2018 Season stats (per game):</h4>
          <p id="value">VALUE: {{value}}</p>
        </div>
      </div>
      <div id="stats">
        <div class="stat-box">
          <h4>FG:</h4>
          <p>{{fg}}%</p>
        </div>
        <div class="stat-box">
          <h4>FT:</h4>
          <p>{{ft}}%</p>
        </div>
        <div class="stat-box">
          <h4>PPG:</h4>
          <p>{{points}}</p>
        </div>
        <div class="stat-box">
          <h4>3P:</h4>
          <p>{{threes}}</p>
        </div>
        <div class="stat-box">
          <h4>AST:</h4>
          <p>{{assists}}</p>
        </div>
        <div class="stat-box">
          <h4>REB:</h4>
          <p>{{rebounds}}</p>
        </div>
        <div class="stat-box">
          <h4>STL:</h4>
          <p>{{steals}}</p>
        </div>
        <div class="stat-box">
          <h4>BLK:</h4>
          <p>{{blocks}}</p>
        </div>
        <div class="stat-box">
          <h4>TO:</h4>
          <p>{{turnovers}}</p>
        </div>
      </div>
    </div>

    <div class="suggested">
      <Suggested :players="matchList" v-bind:name="searchName"/>
    </div>
  </div>
</template>

<script>
import Autocomplete from './Autocomplete';
import Suggested from './Suggested';

export default {
  components: {
    Autocomplete,
    Suggested
  },
  data () {
    return {
      playersList: [],
      playerNames: [],
      matchList: [],
      searchName: '',
      player: '',
      source: 'ballswirl.png',
      fg: '',
      ft: '',
      threes: '',
      points: '',
      assists: '',
      rebounds: '',
      steals: '',
      blocks: '',
      turnovers: '',
      value: '',

    }
  },
  created: function() {
    // full list of NBA players
    /*fetch('https://data.nba.net/10s/prod/v1/2018/players.json')
      .then((res) => res.json())
      .then((data) => {
        this.playersList = data.league.standard;
        //console.log(this.playersList);
      })
      .catch((err) => {
        console.log(err);
      });*/

    // NBA PLAYER STATS & COMPUTED Z-SCORES (WEIGHTED) via FB-Ninja
    // fetch JSON from file
    fetch("players.xlsx")
      .then((res) => {
        // get data as blob
        if(!res.ok) throw new Error ("Players data fetch failed");
        return res.arrayBuffer();
      })
      .then ((ab) => {
        // parse data
        const data = new Uint8Array(ab);
        const XLSX = require('xlsx');
        const wb = XLSX.read(data, {
          type:"array"
        });
        // store data in JSON array
        const sheet = wb.SheetNames[0];
        const playerSheet = wb.Sheets[sheet];
        this.playersList = XLSX.utils.sheet_to_json(playerSheet);
        this.indexNames();
      });

  },

  methods: {
    indexNames() {
      const list = this.playersList;
      list.forEach((player) => {
        let name = player.NAME;
        this.playerNames.push(name);
      });
    },

    playerSelected(player) {
      this.searchName = player;
      this.findPlayer();
    },
    findPlayer() {
      let index = 0;
      this.playersList.forEach((player) => {
        let foundName = player.NAME;
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.getStats(index);
          this.setImg(foundName);
          this.findMatch(index);
        };
        index++;
      });
    },
    getStats(id) {
      const list = this.playersList;
      this.fg = list[id]["FG%"].toFixed(3);
      this.ft = list[id]["FT%"].toFixed(3);
      this.threes = list[id]["3P"].toFixed(2);
      this.points = list[id].PTS.toFixed(2);
      this.assists = list[id].AST.toFixed(2);
      this.rebounds = list[id].REB.toFixed(2);
      this.steals = list[id].STL.toFixed(2);
      this.blocks = list[id].BLK.toFixed(2);
      this.turnovers = list[id].TO.toFixed(2);
      this.value = list[id].VALUE.toFixed(2);
    },

    setImg(name) {
      let firstName = name.split(' ').slice(0, -1).join(' ');
      let lastName = name.split(' ').slice(-1).join(' ');
      this.source = "https://nba-players.herokuapp.com/players/" + lastName + "/" + firstName;
    },

    findMatch(index) {
      let tempList = [];
      if(index < 5) {
        for(let i = 0; i < 10; i++) {
          if(i != index) {
            tempList.push(this.playersList[i]);
          }
        }
      } if(index >= 5) {
        for (let i = (index - 5); i <= (index + 5); i++) {
          if(i != index) {
            this.playersList[i];
            tempList.push(this.playersList[i]);
          }
        }
      }
      this.matchList = tempList;
      console.log(this.matchList);
      this.searchName = '';
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

*:focus {
    outline: none;
}

.search {
  width: 380px;
  margin: auto;
  border: 1px solid $secondary;
  border-radius: 40px;
  transition: all .5s linear;
  background: rgb(255, 255, 255);
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);

}

.search:hover {
  border-radius: 5px;
  transition: all .5s linear;
}

.search-bar {
  margin: 5px auto;
  width: 300px;
  height: 35px;
  border-style: none;
}


.player-card {
  @include flex-col;
  background: rgb(255, 255, 255);
  padding: 20px 40px;
  margin: 40px auto;
  width: 80%;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

#profile {
  @include flex-row;
}

#stats {
  @include flex-row;
  justify-content: space-between;
}

.stat-box {
  width: 100%;
}
#profile-pic {
  width: 33%;
}

#profile-text {
  padding-left: 10px;
  text-align: left;
}
#head-shot {
  width: 100%;
  padding: 5px;
}
.player-card h4 {
  color: #fff;
}

.player-card h3 {
  color: $primary;
}

#value {
  color: $primary;
  font-weight: bold;
}
</style>
