<template>
  <div class="search-input">
    <input type="text" v-model="searchPokemon" placeholder="Search Pokémon" @input="onSearchInput" />
  </div>
  <div class="list">
    <article
      v-for="(pokemon, index) in filteredPokemons"
      :key="pokemon.name"
      @click="setPokemonId(pokemon.url.split('/').filter(Boolean).pop())">
      <img :src="getPokemonImageUrl(pokemon.url)" alt="Pokémon image" />
      <h3>{{ pokemon.name }}</h3>
      <!-- <h3>{{ pokemon.url }}</h3> -->
    </article>

    <div id="scroll-trigger" ref="infinitesscrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
  </div>
</template>

<script >
export default {
  props: ['imageUrl', 'apiUrl','pokemonId'],
  data: () => ({
    pokemons: [],
    filteredPokemons: [],
    nextUrl: '',
    currentUrl: '',
    searchPokemon: '',
    pokemonId: '',
  }),
  methods: {
    fetchData() {
      fetch(this.currentUrl)
        .then((resp) => {
          if (resp.ok) {
            return resp.json();
          }
        })
        .then((data) => {
          console.log(data);
          this.nextUrl = data.next;
          this.pokemons.push(...data.results);
          this.filterPokemons();
        })
        .catch((error) => {
          console.error('Error:', error);
        });
    },
    getPokemonImageUrl(url) {
      const id = url.split("/").filter(Boolean).pop();
      return `${this.imageUrl}${id}.png`;
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });

      observer.observe(this.$refs.infinitesscrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonId(id) {
      // const id = url.split("/").filter(Boolean).pop();
      // this.pokemonId = id; 
      this.$emit('setPokemonId',id);
    },
    filterPokemons() {
      this.filteredPokemons = this.pokemons.filter(pokemon =>
        pokemon.name.toLowerCase().includes(this.searchPokemon.toLowerCase())
      );
    },
    onSearchInput() {
      this.filterPokemons();
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  }
}
</script>

<style lang="scss" scoped>
.list {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
        grid-gap: 10px;
        width: 100%;
        max-width: 510px;

        article {
            height: 150px;
            background-color: #efefef;
            text-align: center;
            text-transform: capitalize;
            border-radius: 5px;
            cursor: pointer;
            box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
                        0 10px 10px rgba(0, 0, 0, .2);

            h3 {
                margin: 0;
            }
        }
  
}

#scroll-trigger {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    font-size: 2rem;
    color: #efefef;
}

.search-input {
        position: relative;
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
            box-shadow: 0 15px 30px rgba(0,0,0,.2),
                        0 10px 10px rgba(0,0,0,.2);
        }

        i {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 1.25rem;
            color: #0A2E50;
            cursor: pointer;
        }
    }

</style>