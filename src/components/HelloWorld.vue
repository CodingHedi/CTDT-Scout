<template>
<div class="hello">
  <h1>{{ msg }}</h1>
  <p>
    For a guide and recipes on how to configure / customize this project,<br>
    check out the
    <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
  </p>
  <Buttonz name="a" v-bind:last="last" v-on:onclick-button="setLast($event)" />
  <Buttonz name="b" v-bind:last="last" v-on:onclick-button="setLast($event)" />
  <Buttonz name="c" v-bind:last="last" v-on:onclick-button="setLast($event)" />

  <h3>🔥 PLUS RIEN N'EST ESSENTIEL ICI !!! 🔥</h3>

  <p>
    Le dernier putain de bouton appuyé est {{ last }}
  </p>
  <br />
  <br />
  <Grid></Grid>
</div>
</template>

<script>
import Buttonz from './Button.vue'
import Grid from './Grid.vue'
export default {
  name: 'HelloWorld',
  components: {
    Buttonz,
    Grid
  },
  data() {
    return {
      last: ''
    }
  },
  methods: {
    setLast(name) {
      this.last = name
    }
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
