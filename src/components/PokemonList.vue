<template>
  <div class="list">
    <article v-for="pokemon in filtered" v-bind:key="pokemon.name" v-on:click="showDetail(pokemon)">
      <h3>{{pokemon.name}}</h3>
      <img v-bind:src='url_image+pokemon.name+".png"'>
    </article>
  </div>
</template>

<script>
import axios from "../../node_modules/axios"
import json from "../config/config.json"
export default {
  beforeMount() {
    // appel de l'api pour récuperer la liste des pokemons
    axios.get(json.API_URL+'/pokemon')
      .then((e) => {
        // sur le retour on stock la data dans pokemons
        this.pokemons = e.data.results;
      })
  },
  
  data: function () {
    return {
      pokemons: "",
      url_image: json.IMG_URL,
    }
  },

  props: {
    searchdata: String,
  },

  methods: {
    showDetail(pokemon) {
      this.$emit("showPokemonDetailEmit", pokemon);
    }
  },

  computed: {
    filtered: function () {
      if (this.searchdata !== "") {
        return this.pokemons.filter(el => el.name.toLowerCase().indexOf(this.searchdata.toLowerCase()) !== -1);
      }
      return this.pokemons;
    }
  }
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 510px;
}
article {
  height: 150px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}

img {
  width: 115px;
  height: 115px;
}
</style>

