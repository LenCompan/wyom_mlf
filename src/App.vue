<template>
  <div id="app" class="container">
    <HelloWorld msg="Make your own murder: Generator" />

    Number of Univers <input v-model.number="nb_univers" type="number" />
    <br />
    Number of Constrains
    <input v-model.number="nb_contraintes" type="number" /> <br />
    <br />
    <button v-on:click="getJson()" class="btn btn-success">
      Generate !
    </button>
    <div class="row">
      <br />
      <PlaceCard v-if="randomUnivers !== null" v-bind:choice="choice" />
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import PlaceCard from "./components/PlaceCard.vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
import json from "./json/data.json";
import axios from "axios";

export default {
  name: "App",
  components: {
    HelloWorld,
    PlaceCard,
  },
  data() {
    return {
      jsonData: json,
      url:
        "https://spreadsheets.google.com/feeds/list/1zY-iozrH3NVgOds2-0AlZu17YojhEsBWu1X1fbHTPSw/1/public/values?alt=json",
      univers: null,
      contraintes: null,
      choice: {
        univers: [],
        contraintes: [],
      },
      randomUnivers: null,
      randomContrainte: null,
      nb_univers: 1,
      nb_contraintes: 1,
    };
  },
  methods: {
    getJson: function() {
      axios.get(this.url).then((res) => {
        this.univers = [];
        this.contraintes = [];

        this.choice = {
          universArray: [],
          contraintesArray: [],
        };

        console.log(this.choice);

        res.data.feed.entry.forEach((el) => {
          if (el.gsx$univers.$t != "") {
            this.univers.push(el.gsx$univers.$t);
          }

          if (el.gsx$contrainte.$t != "") {
            this.contraintes.push(el.gsx$contrainte.$t);
          }
        });

        for (let index = 0; index < this.nb_univers; index++) {
          this.randomUnivers = Math.floor(
            Math.random() * Math.floor(this.univers.length)
          );
          let y = this.univers[this.randomUnivers];
          this.choice.universArray.push(y);
        }

        for (let index = 0; index < this.nb_contraintes; index++) {
          this.randomContrainte = Math.floor(
            Math.random() * Math.floor(this.contraintes.length)
          );
          let x = this.contraintes[this.randomContrainte];
          this.choice.contraintesArray.push(x);
        }

        console.log(this.choice);
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
