<template>
  <div class="detail">
    <div class="detail-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="" />
      </div>
      <div v-if="pokemon" class="data">
        <h2>{{ pokemon.name }} #{{ pokemon.id }}</h2>
        <div class="property">
          <div class="left">Pokedex Index</div>
          <div class="right">#{{ pokemon.id }}</div>
        </div>
        <div class="property">
          <div class="left">Base Experience</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Height</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <div class="left">Weight</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>
        <div class="property">
          <div class="left">Types</div>
          <div class="types">
            <div
              class="type"
              v-for="(value, index) in pokemon.types"
              :key="'value' + index"
            >
              {{ value.type.name }}
            </div>
          </div>
        </div>
        <div class="property">
          <div class="left">Abilities</div>
          <div class="abilities">
            <div
              class="ability"
              v-for="(value, index) in pokemon.abilities"
              :key="'value' + index"
            >
              {{ value.ability.name }}
            </div>
          </div>
        </div>
      </div>
      <button class="close" @click="closeDetail">
        <img class="image-button" src="./assets/button.png" />
      </button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl"],
  data: () => {
    return {
      show: false,
      pokemon: {}
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then(resp => {
          if (resp.status === 200) return resp.json();
        })
        .then(data => {
          this.pokemon = data;
          this.show = true;
          console.log(this.pokemon);
        })
        .catch(error => {
          console.log(error);
        });
    },
    closeDetail() {
      this.$emit("closeDetail");
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  }
};
</script>

<style lang="scss">
.detail {
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: 100%;
  height: 100%;
  background: rgba($color: #000000, $alpha: 0.7);

  .detail-view {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: fixed;
    top: 50%;
    left: 50%;
    margin-top: -300px;
    margin-left: -150px;
    width: 100%;
    max-width: 300px;
    max-height: 600px;
    padding: 50px 0 0;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 15px 30px rgba(0, 0, 0, 0.2);

    .image {
      display: flex;
      justify-content: center;
      align-items: center;
      position: absolute;
      top: -60px;
      width: 120px;
      height: 120px;
      background-color: #333;
      border-radius: 50%;
      overflow: hidden;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
    }
    h2 {
      text-transform: capitalize;
    }
    .data {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin-bottom: 40px;

      .property {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
        margin-bottom: 10px;

        .left {
          float: left;
        }
        .right {
          float: right;
          text-transform: capitalize;
        }
        ul {
          list-style-type: none;
        }
      }
    }
    .close {
      width: 55px;
      height: 55px;
      text-transform: uppercase;
      border-radius: 75%;
      background-color: #333;
      color: #fff;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);

      .image-button {
        width: 75px;
        height: 75px;
        margin-left: -18px;
        margin-top: -13px;
      }
    }
    h3 {
      width: 90%;
      max-width: 400px;
    }
    .types,
    .abilities {
      display: flex;
      justify-content: flex-start;
      flex-wrap: wrap;
      width: 90%;
      max-width: 400px;

      .type,
      .ability {
        margin: 0 10px 10px 0;
        padding: 5px 10px;
        border-radius: 20px;
        color: #fff;
        letter-spacing: 1px;
        font-size: 13px;
        text-decoration: none;
        text-transform: capitalize;
        word-wrap: wrap;
        word-break: keep-all;
      }
      .type {
        background-color: #606b9f;
      }
      .ability {
        background-color: #8d4e05;
      }
    }
  }
}
</style>
