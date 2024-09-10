<template>
    <div class="container">
        <!-- <img src="/src/assets/images.png" alt="Pokeball"> -->
        <h1>Pokemon</h1>
       <PokemonList 
                :imageUrl="imageUrl" 
                :apiUrl="apiUrl"
                @setPokemonId="setPokemonId"/>
        <PokemonDetail 
                :show-detail="showDetail"
                :image-url="imageUrl"
                :pokemon-id="pokemonId"
                @close-detail="closeDetail"/>
    </div>
</template>
  
  <script>
    import PokemonList from './PokemonList.vue';
    import PokemonDetail from './PokemonDetail.vue';
  
    export default {
      data: () => {
        return {
          imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
          apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
          pokemonUrl: '',
          pokemonId: '',
          showDetail: false
        }
      },
      components: {
        PokemonList,
        PokemonDetail
      },
      methods: {
        setPokemonId(id) {
          // this.pokemonUrl = url;
          this.pokemonId  = id;
          console.log(id);
          this.showDetail = true;
        },
        closeDetail() {
            this.showDetail = false;
            this.pokemonUrl = '';
        }
      }
      
    }
    
  </script>
  
  <style lang="scss" scoped>
    @import url('https://fonts.googleapis.com/css?family=Acme');
  
    .container {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 10px;
      width: calc(100% - 20px);
      min-height: calc(100vh - 20px);
      background: radial-gradient(#156F99, #0A2E50);
  
      font-family: 'Acme', arial;
      font-size: 1rem;
      font-weight: normal;
    }
  
    h1 { color: #efefef; }

    img { 
        width: 150px;
        object-fit: cover;
    }

  </style>