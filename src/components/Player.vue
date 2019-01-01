<template lang="html">
  <div class="parent-container">
    <div class="search">
      <input class="search-bar" placeholder="search player" v-model="searchName" @input="findPlayer">
      <Autocomplete :items="playerNames" v-bind:tQuery="searchName" @selected="playerSelected"/>
    </div>
    <div class="found-player">
      <Suggested :players="playerJSON" v-bind:name="searchName"/>
    </div>
    <div class="suggested-players">
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
      playerJSON: [],
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
      this.playerJSON = [],
      this.playersList.forEach((player) => {
        let foundName = player.NAME;
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.getStats(index);
          this.setImg(foundName);
          this.findMatch(index);
          this.playerJSON.push(player);
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

</style>
