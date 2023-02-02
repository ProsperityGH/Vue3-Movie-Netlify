<template>
    <div class="movie-detail">
        <h2>{{ movie.Title }}</h2>
        <h2>{{ movie.Runtime }}</h2>
        <h3>{{ movie.Released }}</h3>
        <h3>{{ movie.Rated }}</h3>
        <h3>{{ movie.Awards }}</h3>
        <h3>IMDB movie rating: {{ movie.imdbRating }}</h3>
        <img :src="movie.Poster" alt="The Movie Poster" class="featured-img ">
        <p>{{ movie.Plot }}</p>
    </div>
</template>

<script>
// @ is an alias to /src

import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

import '../assets/css/style.css'

export default {
    setup () {
        const movie = ref({});
        const route = useRoute();

        onBeforeMount(() => {
            fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
            .then((response) => response.json())
            .then(data => {
                movie.value = data;
                console.log(data);
            });
        });

        return {
            movie
        }
    }
}
</script>
  