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

  created() { 
          // if(this.store.searchText == '') {}
      axios
        .get('https://api.themoviedb.org/3/movie/popular', {
          params: {
            api_key: store.apiKey,
            
          }
        })
        .then((res) => {
          console.log(res.data);
          store.movies = res.data.results;
          console.log(store.movies);
        });

      axios
        .get('https://api.themoviedb.org/3/tv/popular', {
          params: {
            api_key: store.apiKey,
            
          }
        })
        .then((res) => {
          console.log(res.data);
          store.series = res.data.results;
          console.log(store.series);
        });
    
    
      
  },

  methods: {
    search() {
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
        axios
        .get('https://api.themoviedb.org/3/search/tv', {
          params: {
            api_key: store.apiKey,
            query: store.searchText
          }
        })
        .then((res) => {
          console.log(res.data);
          store.series = res.data.results;
          console.log(store.series);
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
      getRoundedVote(number) {
        const roundedVote = Math.ceil(number / 2);
        return roundedVote;
      }
    }
  }
</script>

<template>
  <header class="py-3 bg-dark">
    <div class="container d-flex align-items-center justify-content-between">
      <h1 class="text-danger">
        BOOLFLIX
      </h1>
      <form class="d-flex" @submit.prevent="search()">
        <input class="form-control  me-2" type="text" v-model="store.searchText" placeholder="Cerca un film...">
        <button class="btn btn-light" type="submit">
          <i class="fa-solid fa-magnifying-glass"></i>
        </button>
      </form>
      
    </div>
  </header>
  <main>

    <div class="container">
      
          <h2 class="py-2">Film</h2>
          <ol>
            <div class="row">
              <div v-for="(movie, i) in store.movies" :key="i" class="col-3">
                <li class="my-3 card">
                  <img class="img-fluid card-img-top" :src="'https://image.tmdb.org/t/p/w185' + movie.poster_path" :alt="movie.title">
                  <ul class="card-body">
                      <li>
                        Titolo: {{ movie.title }}
                      </li>
                      <li>
                        Titolo originale: {{ movie.original_title }}
                      </li>
                      <li>
                        <!-- Lingua: {{ movie.original_language }} -->
                        Lingua: <img class="lang-flag" :src="getFlag(movie.original_language)" alt="">
                      </li>
                      <li>
                        <span v-for="x in (getRoundedVote(movie.vote_average))" :key="x">
                          <i class="fa-solid fa-star text-warning"></i>
                        </span>
                        <span v-for="x in (5 - (getRoundedVote(movie.vote_average)))" :key="x">
                          <i class="fa-regular fa-star text-warning"></i>
                        </span>
                      </li>
                  </ul>
                </li>
              </div>
            </div>
            
          </ol>
        
    </div>

    <hr>

    <div class="container">
      
          <h2 class="py-2">Serie TV</h2>

          <ol>
            <div class="row">
              <div  v-for="(serie, i) in store.series" :key="i" class="col-3">
                <li class="my-3 card">
                    <img class="img-fluid card-img-top" :src="'https://image.tmdb.org/t/p/w185' + serie.poster_path" :alt="serie.name">
                    <ul class="card-body">
                      <li>
                        Titolo: {{ serie.name }}
                      </li>
                      <li>
                        Titolo originale: {{ serie.original_name }}
                      </li>
                      <li>
                        Lingua: <img class="lang-flag" :src="getFlag(serie.original_language)" alt="">
                      </li>
                      <li>
                        <span v-for="x in (getRoundedVote(serie.vote_average))" :key="x">
                          <i class="fa-solid fa-star text-warning"></i>
                        </span>
                        <span v-for="x in (5 - (getRoundedVote(serie.vote_average)))" :key="x">
                          <i class="fa-regular fa-star text-warning"></i>
                        </span>
                      </li>
                    </ul>

                </li>
              </div>
            </div>
            

          </ol>
        
    </div>
  </main>
</template>

<style lang="scss">
@use 'assets/scss/main' as *;
@import "bootstrap/scss/bootstrap";

img.lang-flag {
    max-width: 35px;
}

ol,
ul {
    list-style: none;
    padding: 0;
    
}
.card .card-body {
  display: none;
}
.card:hover .card-body {
  display: block;

}



</style>
