<script>
/* 
  Per importare ed utilizzare un componente dentro un altro devo SEMPRE seguire questi 3 passi:
  1) Importazione del componente
  2) Dichiarazione del componente
  3) Utilizzo del componente
*/
// 1) Importazione del componente
import {store} from './store.js';
import axios from 'axios';
// import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return { 
      //store: store,
      store,
    }
  },
  // 2) Dichiarazione del componente
  components: {
   
  },
  methods: {
    search() {

        console.log('SEARCHING: ', store.searchText);

        axios
        .get('https://api.themoviedb.org/3/search/movie', {
          params: {
            api_key: store.apiKey,
            query: store.searchText
          }
        })
        .then((res) => {
          console.log(res.data);
          store.movies = res.data.results;
          console.log(store.movies);
        });
      },
      getFlag(lang) {
        const validLangs = {
          de: 'de.png',
          en: 'en.png',
          es: 'es.png',
          fr: 'fr.png',
          it: 'it.png',
          ja: 'jp.png',
          pt: 'pt.png'
        };

        if(lang in validLangs) {
         return '/img/' + validLangs[lang]
          
        }
        else {
          return '/img/eu.png'
        }
      },
    }
  }
</script>

<template>
  <header>
    <div>
      <h1 class="bg-warning">
        BIOPARCO
      </h1>
      <form @submit.prevent="search()">
        <input type="text" v-model="store.searchText" placeholder="Cerca un film...">
        <button type="submit">
          Cerca
        </button>
      </form>
      <!-- <h1>
        {{ store.searchText }}
      </h1> -->
    </div>
  </header>
  <main>
    <ol>
      <li v-for="(movie, i) in store.movies" :key="i">
        <ul>
          <li>
            Titolo: {{ movie.title }}
          </li>
          <li>
            Titolo originale: {{ movie.original_title }}
          </li>
          <li>
            <!-- Lingua: {{ movie.original_language }} -->
            Lingua: <img :src="getFlag(movie.original_language)" alt="">
          </li>
          <li>
            Voto: {{ movie.vote_average }}
          </li>
        </ul>

        <hr>

      </li>
    </ol>
  </main>
</template>

<style lang="scss">
@use 'assets/scss/main' as *;
@import "bootstrap/scss/bootstrap";
</style>
