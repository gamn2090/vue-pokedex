<template>
<div class="w-full flex justify-center">
  <input type="text" placeholder="Enter Pokemon Here"
    class="mt-10 p-2 border-blue-500 border-2"
    v-model="text">

</div>
<div class="mt-10 p-4 flex flex-wrap justify-center">
  <div class="ml-4 text-2x text-blue-500"
    v-for="(pokemon, idx) in filteredPokemon"
    :key="idx">
      <router-link :to="`/Pokemon/${urlIdLooUp[pokemon.name]}`">
        {{pokemon.name}}
      </router-link>
  </div>
</div>
  
</template>

<script>
// @ is an alias to /src

import {computed, reactive, toRefs} from 'vue';

export default {
  
  name: 'HomeView',
  setup(){
    const state = reactive ({
      pokemons: [],
      urlIdLooUp: {},
      text: "",
      filteredPokemon: computed(()=> updatePokemon())
    })

    function updatePokemon(){
      if(!state.text){
        return []
      }

      return state.pokemons.filter((pokemon)=>
        pokemon.name.includes(state.text)
      )
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0&limit=2000")
      .then((res)=> res.json())
      .then((data)=> {
        console.log(data)
        state.pokemons = data.results;
        // state.urlIdLooUp = data.results.reduce((acc, cur, idx)=>          
        //   acc= {...acc, [cur.name]:[cur.url]}
        // ,{})
        state.urlIdLooUp = data.results.reduce((acc, cur, idx) => {
          // Extraer el ID del URL utilizando expresiones regulares
          const realId = cur.url.match(/\/pokemon\/(\d+)\/$/);
          
          if (realId) {
            // Si se encuentra una coincidencia, asignar el ID extraído
            acc[cur.name] = parseInt(realId[1]);
          } else {
            // Si no se encuentra una coincidencia, asignar un valor predeterminado (puedes manejarlo según tus necesidades)
            acc[cur.name] = idx+1;
          }
          
          return acc;
      }, {});
      })

      return {... toRefs(state)}
  } 
}
</script>
