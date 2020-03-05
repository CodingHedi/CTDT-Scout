<template>
<div>
  <div class="filter">
    <select v-model="selected">
      <option disabled value="">Choisissez</option>
      <option>{{ typeForColor.bleu }}</option>
      <option>{{ typeForColor.rouge }}</option>
      <option>{{ typeForColor.vert }}</option>
    </select>
    <span> Sélectionné : {{ selected }}</span>
    <br />
    <br />
  </div>
  <div class="item-container">
    <div class="autocomplete">
      <input type="text" v-model="search" @input="onChange" />
      <ul v-show="isOpen" class="autocomplete-results">
        <li v-for="(item) in filteredPlayerslist" v-bind:key="item.id" @click="setResult(item.name)" class="autocomplete-result">
          {{ item.name }}
        </li>
      </ul>
    </div>
    <div v-for="item in filteredPlayerslist" v-bind:key="item.id" class="item">
      {{ item.name }}
      <br />
      <img v-bind:src="image(item.desc.img)" />
      <img v-bind:src="imgSrcForColor(item.desc.color)" class="reduce" />
    </div>
  </div>
</div>
</template>

<script>
import json from '../data/fakedata.json'

const images = require.context('../assets/CTDT_mini_card/', false, /\.gif$/);
const types = require.context('../assets/', false, /\.png$/);
const typeForColor = {
  "bleu": "Vit",
  "rouge": "Force",
  "vert": "Tech"
};

export default {
  name: 'Grid',
  components: {},
  computed: {
    filteredPlayerslist() {
      let currentPlayers = this.playerJSON.players;
      currentPlayers = this.filteredPlayersByType(currentPlayers)
      //currentPlayers = this.filteredPlayersByRegion(currentPlayers)
      //currentPlayers = this.filteredPlayersByRarity(currentPlayers)
      currentPlayers = this.filteredPlayersByName(currentPlayers)
      return currentPlayers
    }
  },
  methods: {

    filteredPlayersByName(currentPlayers) {
      return currentPlayers.filter(item => item.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1)
    },

    setResult(item) {
      this.search = item;
      this.isOpen = false;
    },

    filteredPlayersByType(currentPlayers) {
      return currentPlayers.filter(p => this.type(p.desc.color) === this.selected || this.selected === '')

    },

    type(color) {
      color = color.toLowerCase()
      if (color in typeForColor) {
        return typeForColor[color]
      } else {
        alert("C'EST LA MERDE FFS ! " + color)
      }
    },

    imgSrcForColor(color) {
      const type = this.type(color)
      return types(`./${type}.png`)
    },

    image(id) {
      return images(`./${id}.gif`)
    },
    onChange() {
      this.isOpen = true;
      if (this.search === '') {
        this.isOpen = false;
      }
    }
  },
  data() {
    return {
      playerJSON: json,
      selected: '',
      search: '',
      typeForColor: typeForColor,
      isOpen: false
    }
  }
}
</script>

<style>
.item-container {
  width: 80%;
  margin-left: auto;
  margin-right: auto;

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.item {
  width: 128px;
  height: 128px;
  margin-bottom: 10px;

  flex-grow: 1 2 3;
}

.reduce {
  width: 18%;
  height: 18%;
}

.autocomplete {
  position: relative;
  width: 130px;
}

.autocomplete-results {
  padding: 0;
  margin: 0;
  border: 1px solid #eeeeee;
  height: 120px;
  overflow: auto;
}

.autocomplete-result {
  list-style: none;
  text-align: left;
  padding: 4px 2px;
  cursor: pointer;
}

.autocomplete-result:hover {
  background-color: #4AAE9B;
  color: white;
}
</style>
