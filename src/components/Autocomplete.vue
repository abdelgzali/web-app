<template lang="html">
  <div class="autocomplete">
    <div class="options" v-show="visible" v-model="query">
      <ul>
        <li v-for="(match, index) in matches" @click="clicked(index)">
          {{match}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ['items', 'tQuery'],
  data() {
    return {
      visible: false,
      query: '',
      selected: null,
    };
  },
  methods: {
    toggle() {
      this.visible = !this.visible;
    },
    clicked(index) {
      this.selected = this.matches[index];
      this.$emit('selected', this.selected);
      this.visible = false;
      this.query = '';
    }
  },
  computed: {
    matches() {
      this.query = this.tQuery;
      if(this.query == '') {
        this.visible = false;
      }
      if (this.query.length > 2) {
        this.visible = true;
        console.log("finding player...");
        let queryList = this.items.filter((item) => item.toLowerCase().includes(this.query.toLowerCase()));
        if (queryList.length < 1) {
          console.log("none");
          let none = ["No matches found"];
          return none
        } else {
          return queryList
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/scss/_mixins.scss";

ul {
  list-style: none;
  text-align: left;
  padding-left: 0;
  margin: 0;
  position: absolute;
  background-color: #fff;
  width: 100%;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);

}

li {
  padding: 10px 0 10px 40px;
}

li:hover {
  background-color: #ecf0f1;
  cursor: pointer;
}

.autocomplete {
  width: 100%;
  position: relative;
}

.input {
  height: 100px;
}

.options {
  border-top: 1px solid #ecf0f1;
  transition: all .3s linear;
}

@media only screen and (max-width: 48em) {
  .search {
    width: calc(100% - 4.8em);
  }
}

</style>
