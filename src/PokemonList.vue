<template>
  <div class="cont">
    <div class="searchbar">
      <input type="text" v-model="searchvalue" @keyup="search" />
    </div>

    <div class="list">
      <article
        v-for="(pokemon, index) in filteredPokemons"
        :key="'poke' + index"
        @click="setPokemonUrl(pokemon.url)"
      >
        <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" />
        <h3>{{ pokemon.name }}</h3>
        <h4>#{{ pokemon.id }}</h4>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
      searchvalue: "",
      filteredPokemons: []
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then(resp => {
          if (resp.status === 200) return resp.json();
        })
        .then(response => {
          console.log(response);
          this.nextUrl = response.next;
          response.results.forEach(pokemon => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function(part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
          this.filteredPokemons = this.pokemons;
        })
        .catch(error => {
          console.log(error);
        });
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonUrl(url) {
      this.$emit("setPokemonUrl", url);
    },
    search() {
      this.filteredPokemons = this.pokemons.filter(pokemon =>
        pokemon.name.includes(this.searchvalue)
      );
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  }
};
</script>

<style lang="scss">
.cont {
  text-transform: capitalize;
  display: block;
  width: 100%;
  .list {
    position: relative;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 20px;
    width: 100%;
    max-width: 65%;

    article {
      height: 200px;
      background-color: #fff;
      text-align: center;
      border-radius: 5px;
      box-shadow: 5px 10px #111, 5px 10px #111;
      h3 {
        margin: 0;
      }
      cursor: pointer;
    }
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
  .searchbar {
    position: relative;
    margin: 0 auto;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;
    input {
      border: none;
      outline: none;
      border-radius: 5px;
      padding: 10px 40px 10px 10px;
      width: calc(100% - 50px);
      font-size: 1rem;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
    }
    i {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 1.25rem;
      color: #0a2e50;
      cursor: pointer;
    }
  }
  h4 {
    font-size: 23px;
  }
}
</style>
