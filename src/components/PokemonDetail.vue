<template>
    <div>
        <div class="detail" v-if="showDetail">
        <div class="detail-view">
            <div  class="image" v-if="pokemon">
                <!-- <img :src="`${imageUrl}${pokemon.id}.png`" alt="Pokemon Image" /> -->
                <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`" alt="Pokemon Image" />
            </div>
            <div  class="data" v-if="pokemon">
                <h2>{{ pokemon.name }}</h2>
                
                <div class="property">
                    <div class="left">Base Experience</div>
                    <div class="right">{{ pokemon.base_experience }} XP</div>
                </div>
                <div class="property">
                    <div class="left">Height</div>
                    <div class="right">{{ pokemon.height / 10 }} m</div>
                </div>
                <div class="property">
                    <div class="left">Weigth</div>
                    <div class="right">{{ pokemon.weight / 10 }} kg</div>
                </div>
                <h3>Pokemon Types</h3>
                <div class="types">
                    <div class="type" 
                        v-for="(value, index) in pokemon.types"
                        :key="'value'+index">
                        {{ value.type.name }}
                    </div>
                    </div>
                    <h3>Abilities</h3>
                    <div class="abilities">
                    <div class="ability" 
                        v-for="(value, index) in pokemon.abilities"
                        :key="'value'+index">
                        {{ value.ability.name }}
                    </div>
                    </div>
                </div>
                <h2 v-else>The pokemon was not found</h2>
                <!-- Navigation Buttons -->
                <div class="navigation">
                    <button class="close" @click="prevPokemon" :disabled="pokemon.id === 1">Previous</button>
                    <button class="close" @click="closeDetail">Close</button>
                    <button class="close" @click="nextPokemon">Next</button>
                </div>
        </div>
                <!-- <i v-else class="fas fa-spinner fa-spin"></i> -->
    </div>
    </div>
   
</template>

<script >
export default {
  props: ["imageUrl", "pokemonId", "showDetail"],
  name: "pokemon-detail",
  data() {
    return {
        pokemon:null,
        currentPokemonId: null,
    };
  },
  methods: {
    fetchData(pokemonId) {
      if (!pokemonId) return;
      
      const url = `https://pokeapi.co/api/v2/pokemon/${pokemonId}`;
      fetch(url)
        .then((resp) => {
          if (resp.ok) return resp.json();
          return Promise.reject(new Error(`API request failed with status ${resp.status}`));
        })
        .then((data) => {
          this.pokemon = data;
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    closeDetail() {
      this.$emit("closeDetail");
    },
    nextPokemon() {
      this.currentPokemonId += 1;
      this.fetchData(this.currentPokemonId);
    },
    prevPokemon() {
      if (this.currentPokemonId > 1) {
        this.currentPokemonId -= 1;
        this.fetchData(this.currentPokemonId);
      }
    },
  },
  watch: {
    pokemonId: {
      immediate: true,
      handler(newId) {
        this.currentPokemonId = Number(newId);
        this.fetchData(this.currentPokemonId);
      },
    },
  },
};
</script>

<style lang="scss" scoped>
  .detail {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    position: fixed;
    top: 0;
    left: 0;
    padding: 90px 10px 10px;
    width: calc(100% - 20px);
    height: calc(100vh - 20px);
    background: rgba($color: #000000, $alpha: .7);

    .detail-view {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative;
      width: 100%;
      max-width: 510px;
      padding: 50px 0 0;
      background-color: #fff;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);
    
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
        box-shadow: 0 15px 30px rgba(0,0,0,.2),
                    0 10px 10px rgba(0,0,0,.2);
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

          .left { float: left; }
          .right { float: right; }
        }

        h3 {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
        }

        .types, .abilities {
          display: flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          width: 90%;
          max-width: 400px;

          .type, .ability {
            margin: 0 10px 10px 0;
            padding: 5px 10px;
            border-radius: 20px;
            color: #fff;
            font-size: 1rem;
            letter-spacing: 2px;
            text-transform: capitalize;
            word-wrap: none;
            word-break: keep-all;
          }

          .type { background-color: #0A2E50; }
          .ability { background-color: #C73015; }
        }
      }

      .close {
        outline: none;
        border: none;
        border-radius: 5px;
        background-color: #333;
        color: #efefef;
        padding: 10px 20px;
        margin-bottom: 20px;
        font-size: 1.2rem;
        cursor: pointer;
      }
    }

    i {
      font-size: 2rem;
      color: #efefef;
    }

    .navigation {
        display: flex;
        justify-content: space-between; /* Space between the buttons */
        align-items: center;
        margin-top: 20px;
        }

        button.close {
        background-color: #f44336; /* Red background for the button */
        border: none;
        color: white;
        padding: 10px 20px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 4px 2px;
        cursor: pointer;
        border-radius: 5px;
        transition: background-color 0.3s ease;
        }

        button.close:hover {
        background-color: #d32f2f; /* Darker red on hover */
        }

        button.close:disabled {
        background-color: #9e9e9e; /* Disabled state */
        cursor: not-allowed;
        }


  }
</style>
