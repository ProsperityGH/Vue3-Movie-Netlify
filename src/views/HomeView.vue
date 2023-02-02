<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://static.wikia.nocookie.net/gmod/images/9/99/The_Missing_textures.png/revision/latest?cb=20210208200840" 
        alt="Movie Placeholder" class="featured-img" />
        <div class="detail">
          <h3>Test</h3>
          <p>Test.com</p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What would you like to see?" v-model="search">
      <select name="type" value="Select Type" id="type" v-model="type">
        <option disabled value="">Select Type</option>
        <option value="">Everything</option>
        <option value="movie">Movies</option>
        <option value="series">Series</option>
      </select>
      <input type="submit" value="Search">
    </form>

    <div class="movies-list"> 
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template> 

<script>
// @ is an alias to /src

import { ref } from 'vue';
import env from '@/env.js';

import '../assets/css/style.css'

export default {
  setup () {
    const search = ref("");
    const type = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}&type=${type.value}`)
        .then(response => response.json())
        .then(data => {
          movies.value = data.Search;
          search.value = "";
          console.log(type);

        });
      }
    }

    return {
      search,
      movies,
      type,
      SearchMovies,
    }
  }
}
</script>