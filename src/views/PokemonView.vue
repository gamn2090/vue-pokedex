<template>
  <div class="about">
    <div v-if="pokemon"
      class="w-3/12 m-auto bg-gray-300 mt-4 shadow-2xl flex justify-center flex-col items-center">
      <h3 class="text-2xl text-green-800 uppercase">{{pokemon.name}}</h3>
      <h5 class="text-1xl text-green-800 uppercase">default </h5>
      <div class="flex justify-center items-center">        
        <img v-if="pokemon.sprites.front_default" class="w-48" :src="pokemon.sprites.front_default" alt="front">
        <img v-if="pokemon.sprites.back_default" class="w-48" :src="pokemon.sprites.back_default" alt="back"> 
      </div>
      <h5 v-if="pokemon.sprites.front_shiny" class="text-1xl text-green-800 uppercase">shiny </h5>
      <div class="flex justify-center">        
        <img v-if="pokemon.sprites.front_shiny" class="w-48" :src="pokemon.sprites.front_shiny" alt="front">
        <img v-if="pokemon.sprites.back_shiny" class="w-48" :src="pokemon.sprites.back_shiny" alt="back">        
      </div>
      <h3 class="text-red-400">Types</h3>
      <div v-for="(type,idx) in pokemon.types" :key="idx">
        <h5 class="text-blue-900">{{type.type.name}}</h5>
      </div>

    </div>
    <div class="w-3/12 m-auto bg-gray-300 mt-4 shadow-2xl flex justify-center flex-col items-center">
      <router-link :to="`/`">
        <h4 >return to search</h4>
      </router-link>
    </div>
  </div>
</template>

<script>
import { useRoute } from 'vue-router'
import {reactive, toRefs} from 'vue'

export default {
  setup() {
    const route = useRoute()
    
    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res)=> res.json())
      .then((data)=> {
        //console.log(data)
        state.pokemon = data;
      })

      return { ...toRefs(state)}

  },
}
</script>
