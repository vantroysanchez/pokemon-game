<template>
  <div v-if="!pokemon">Cargando...</div>
  <div v-else>
    <h1>¿Quien es este pokémon?</h1>
    <pokemon-picture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <pokemon-options :pokemons="pokemonGroup" @selection="checkAnswer" />

    <template v-if="showAnswer">
      <h3>{{ message }}</h3>
      <button @click="newGame">Volver a jugar</button>
    </template>
  </div>
</template>

<script>
import PokemonOptions from "@/components/PokemonOptions.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";
import getPokemonOptions from "@/helpers/getPokemonOptions";

export default {
  components: { PokemonOptions, PokemonPicture },
  data() {
    return {
      pokemonGroup: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemons() {
      this.pokemonGroup = await getPokemonOptions();

      const randomNumber = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonGroup[randomNumber];
    },

    checkAnswer(pokemonId) {
      this.showPokemon = true;
      this.showAnswer = true;

      if (pokemonId === this.pokemon.id) {
        this.message = `¡Correcto! es ${(this, this.pokemon.name)}`;
      } else {
        this.message = `Fallaste... era ${(this, this.pokemon.name)}`;
      }
    },

    newGame() {
      window.location.reload();
    },
  },
  mounted() {
    this.mixPokemons();
  },
};
</script>
<style scoped></style>
