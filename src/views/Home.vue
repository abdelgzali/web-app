<template lang="html">
  <div :class="{
      'home-container': matchList.length < 1,
      'home-container-selected': matchList.length >= 1
    }">
    <div class="nav-bar">
      <Nav/>
    </div>
    <div :class="{
        'bg': searchName.length < 3,
        'bg-selected': searchName.length >= 3 || matchList.length > 0
      }">
      <div :class="{
          'big-text': searchName.length < 3,
          'big-text-selected': searchName.length >= 3 || matchList.length > 0
        }">
        <p id="title">Need fantasy basketball trade recommendations?</p>
        <p id="sub-title">Pick a player you would like to trade, and we will help you find suggestions</p>
      </div>
      <div class="search">
        <input
          :class="{
            'search-bar': searchName.length < 1,
            'search-bar-focus': searchName.length >= 1
          }"
          placeholder="search active NBA players (ex. Lebron)"
          v-model="searchName"
          @keyup="searchName = $event.target.value; findPlayer"
          type="search"
          @focus="scrollToTop">
        <Autocomplete
          :items="playerNames"
          v-bind:tQuery="searchName"
          @selected="playerSelected"/>
      </div>
    </div>
    <div class="arrow-container">
      <i class="material-icons" id="arrow-down" v-if="arrow">keyboard_arrow_down</i>
    </div>
    <div class="suggested-players">
      <Playercards :players="matchList" v-bind:name="searchName"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Autocomplete from '@/components/Autocomplete';
import Playercards from '@/components/Playercards';
import Nav from '@/components/Nav';

export default {
  name: 'home',
  components: {
    Autocomplete,
    Playercards,
    Nav
  },
  data () {
    return {
      playersList: [],
      playerNames: [],
      matchList: [],
      searchName: '',
      playerJSON: [],
      player: '',
      arrow: false

    }
  },
  created: function() {
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
      this.toggleArrow();
      setTimeout(() => {
        return this.toggleArrow();
      }, 5000);
    },

    findPlayer() {
      let index = 0;
      this.playersList.forEach((player) => {
        let foundName = player.NAME;
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.findMatch(index);
        };
        index++;
      });
    },

    findMatch(index) {
      let list = this.playersList;
      let searchedPlayer = this.playersList[index];
      list.sort((firstPlayer, secondPlayer) => {
        let firstVal = Math.abs(firstPlayer.VALUE - searchedPlayer.VALUE);
        let secondVal = Math.abs(secondPlayer.VALUE - searchedPlayer.VALUE);
        if (firstVal > secondVal) {
          return 1;
        } else if (firstVal < secondVal) {
          return -1
        } else {
          return 0;
        }
      })
      this.matchList = list.slice(0,11);
      this.searchName = '';
    },
    toggleArrow() {
      this.arrow = !this.arrow;
    },
    scrollToTop() {
      window.scrollTo(0,0);
    }
  }
}
</script>

<style lang="scss">
@import "@/scss/_mixins.scss";
@import "@/scss/_variables.scss";

*:focus {
    outline: none;
}
.home-container {
  height: 100vh;
}
.home-container-selected {
  height: auto;
}

#title {
  color: #fff;
  font-family: sans-serif;
  font-weight: bold;
  font-size: 24px;
  margin: 0;
}
#sub-title {
  font-family: Montserrat, sans-serif;
  font-size: 12px;
}

.bg {
  width: 100%;
  height: calc(100vh - 30px);
  display: flex;
  justify-content: center;
}

.bg-selected {
  width: 100%;
  height: 35vh;
  display: flex;
  justify-content: center;

  .search {
    top: 10em;
  }
}

.big-text {
  position: absolute;
  top: 35%;
  text-align: center;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
  margin: 0;
  padding: 10px 30px;
  color: #bdc3c7;
}

.big-text-selected {
  display: none;
}
.search {
  @include fade-div;
  position: absolute;
  top: 50%;
  left: 50%;
  width: calc(100% - 40vw);
  overflow: visible;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  margin: auto;
  box-sizing: border-box;
  border: 1px solid $secondary;
  border-radius: 30px;
  background: rgb(255, 255, 255);
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.search-bar {
  width: 100%;
  padding: 5px 20px;
  border-radius: 30px;
  height: 40px;
  border-style: none;
}

.search-bar-focus {
  @include fade-div;
  width: 100%;
  padding: 5px 16px;
  height: 35px;
  border-style: none;
  border-radius: 0px;

}

#arrow-down {
  color: #fff;
  padding-bottom: 20px;
  animation: animated-bounce 2s ease;
}

.arrow-container {
  height: 30px;
  padding: 10px 0;
  margin-top: 20px;
}

@media only screen and (max-width: 48em) {
  .home-container {
    height: 100vh;
  }
  .bg-selected {
    height: 12em;
  }
  .big-text {
    top: 35%;
  }
  .search {
    top: 50%;
    width: calc(100% - 4.8em);
  }
}

@media only screen and (max-height: 28em ) {
  .big-text {
    top: 17em;
  }
}

@media only screen and (min-width: 80em) {
  .bg-selected {
    height: 25vh;
  }
  .search {
    width: calc(100% - 60vw);
  }
}

</style>
