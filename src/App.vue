<template>
  <div class="container">
    <img
      alt="pokedex logo"
      src="./assets/pokedex.png"
      width="100px"
      height="100px"
    />
    <h1>Gotta catch 'em all! Pokemon!</h1>
    <PokemonList
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"
    />
    <PokemonDetail
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>

<script>
import PokemonList from "./PokemonList.vue";
import PokemonDetail from "./PokemonDetail.vue";

export default {
  data: () => {
    return {
      imageUrl:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/?limit=964",
      pokemonUrl: "",
      showDetail: false
    };
  },
  components: {
    PokemonList,
    PokemonDetail
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.showDetail = false;
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue");

.container {
  font-family: "Bebas Neue", cursive;
  letter-spacing: 2px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 10px;
  width: calc(100vh- 20px);
  min-height: calc(100vh - 20px);
  background-image: url("./assets/pokeback2.png");
  background-repeat: no-repeat;
  background-size: 100%;
  background-attachment: fixed;
}
h1 {
  font-family: "Bebas Neue", cursive;
  color: #111111;
  margin: 0 auto;
  text-align: center;
}
</style>
