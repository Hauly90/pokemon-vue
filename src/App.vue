<template>
  <div>
    <h1>Welcome to the Pokémon Encyclopedia!</h1>
    <!-- <a v-if="showPokemon">{{ pokemonDetails }}</a> -->
    <!-- <a v-if="showPokemon">{{ anotherAPICall.forms[0].name }}</a> -->

    <div class="from-group gap-1 d-grid justify-content-center">
      <label>Search for pokemon:</label>
      <input
        type="text"
        class="form-control"
        name="onePokemons"
        id="onePokemon"
        v-model="onePokemonValue"
      />
      <button
        type="submit"
        v-on:click="getOnePokemon()"
        class="btn btn-primary"
      >
        Search
      </button>
      <!-- <p> {{ onePokemonValue }} </p> -->
    </div>

    <div v-if="showOnePokemon" class="pokemon_rendering">
      <img :src="onePokemonArray.sprites.other.dream_world.front_default" />
      <p>{{ onePokemonArray.forms[0].name }}</p>
    </div>

    <div class="d-grid gap-2 d-md-flex justify-content-center">
      <button v-on:click="showPokemon = true" class="btn btn-dark">
        Click in order to get pokemons!
      </button>
      <button v-on:click="showPokemon = false" class="btn btn-dark">
        Hide pokemons
      </button>
    </div>

    <div
      v-for="item in anotherAPICall"
      :key="item.id"
      v-if="showPokemon"
      class="pokemon_rendering"
    >
      <img :src="item.sprites.other.dream_world.front_default" />
      <p>{{ item.forms[0].name }}</p>
    </div>
  </div>
</template>

<script>
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemonDetails: [],
      anotherAPICall: [],
      showPokemon: false,
      onePokemonValue: "",
      onePokemonArray: [],
      showOnePokemon: false,
    };
  },
  methods: {
    handleErrors(response) {
      if (!response.ok) {
        throw Error(response.statusText);
      }
      return response.json();
    },
    setAllPokemons() {
      fetch("https://pokeapi.co/api/v2/pokemon/?limit=16")
        .then((response) => this.handleErrors(response))
        .then((data) => {
          this.pokemonDetails = data;

          let promises = data.results.map((pokemon) => fetch(pokemon.url));

          let test = [];

          Promise.all(promises).then((results) =>
            results.forEach((result) => {
              return fetch(result.url)
                .then((data) => data.json())
                .then((result) => test.push(result));
            })
          );

          setTimeout(() => {
            console.log(test);
            this.anotherAPICall = test;
          }, 900);
        });
    },

    getOnePokemon() {
      console.log(this.onePokemonValue);
      // let somePokemon = document.getElementById("onePokemon").value;
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.onePokemonValue}`)
        .then((response) => this.handleErrors(response))
        .catch(error => console.log(error))
        .then((data) => (this.onePokemonArray = data));
      this.showOnePokemon = true;
    },
  },
  getAllPokemons() {},
  created() {
    this.setAllPokemons();
  },
};
</script>

<style>
@import "./assets/base.css";

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  /* text-align: center; */
  font-weight: normal;
}

div .pokemon_rendering {
  border-style: solid;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  margin: 10px;
  border-radius: 20px;
  /* display: table; */
  /* display: inline-block; */
}

h1 {
  text-align: center;
}

button {
  background-color: green;
  color: white;
  border-radius: 5px;
  margin: 5px;
  padding: 5px;
}

input,
label {
  display: block;
}

label {
  font-size: 20px;
}
</style>
