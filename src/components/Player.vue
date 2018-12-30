<template lang="html">
  <div class="parent-container">
    <input class="search-bar" placeholder="search player" v-model="searchName" @input="findPlayer">
    <div class="player-card">
      <h3>{{player}}</h3>
      <img v-bind:src="source" alt="head-shot">
      <h4>2018 Season stats (per game):</h4>
      <p id="value">VALUE: {{value}}</p>
      <p>FG: {{fg}}%</p>
      <p>FT: {{ft}}%</p>
      <p>PPG: {{points}}</p>
      <p>THREES: {{threes}}</p>
      <p>Assists: {{assists}}</p>
      <p>Rebounds: {{rebounds}}</p>
      <p>Steals: {{steals}}</p>
      <p>Blocks: {{blocks}}</p>
      <p>Turnovers: {{turnovers}}</p>
    </div>
    <table id="table">

    </table>
  </div>
</template>

<script>

export default {
  data () {
    return {
      playersList: [],
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
        console.log(this.playersList);
      })
  },
  methods: {
    findPlayer() {
      const list = this.playersList;
      let index = 0;
      list.forEach((player) => {
        let foundName = player.NAME;
        // fuzzy search: foundName.toLowerCase().indexOf(this.searchName.toLowerCase()) !== -1
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.getStats(index);
          this.setImg(foundName);

        };
        index++;
      })
    },
    getStats(id) {
      const list = this.playersList;
      console.log(list[id]);
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
    }
  }
}
</script>

<style lang="scss">
$primary: #F21A13;
$secondary: #1D428A;

*:focus {
    outline: none;
}

.parent-container {
}

.search-bar {
  padding: 5px 40px;
  margin: 20px auto;
  width: 300px;
  height: 35px;
  border: 1px solid $secondary;
  border-radius: 40px;
}


.player-card {
  background: #ecf0f1;
  padding: 20px 40px;
  margin: 20px auto;
  width: 300px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}
.parent-container h4 {
  color: $secondary;
}

.parent-container h3 {
  color: $primary;
}

#value {
  color: $primary;
  font-weight: bold;
}
</style>
