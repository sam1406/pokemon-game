<template>
  <div v-if="!pokemon">
    <h1>Espere un momento porfa</h1>
  </div>
  <div v-else>
    <h1>Quien es este pokemon</h1>
    <!-- imagen del pokemon -->
    <Image :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <!-- opciones de pokemon -->
    <Option :pokemons="pokemonArr" @selection="checkAnswer" />
    <template v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import Image from "@/components/PokemonPicture.vue";
import Option from "@/components/PokemonOption.vue";

import getPokemonOptions from "@/helpers/getPokemonOptions";
export default {
  components: {
    Image,
    Option,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();
      let ranInt = Math.floor(Math.random() * this.pokemonArr.length);
      this.pokemon = this.pokemonArr[ranInt];
      console.log(this.pokemonArr);
    },
    checkAnswer(selectId) {
      if (selectId === this.pokemon.id) {
        this.showPokemon = true;
        this.message = "Correcto";
        this.showAnswer = true;
      }
    },
    newGame() {
      this.showPokemon = false;
      this.showAnswer = false;
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>
