<template>
  <section class="movie-page">
    <div v-if="isQuerying">
      <p class="loading">
        <span class="text"> Loading...</span>
      </p>
    </div>
    <div v-else>
      <div v-if="movieToDetail">
        <MovieDetails
          :movieData="movieToDetail"
          :onAddMovieToFavouritesClick="onAddMovieToFavouritesClick"
          :onRemoveMovieFromFavouritesClick="onRemoveMovieFromFavouritesClick"
        />
      </div>
      <div v-else>
        <p class="warning">Movie not found. It may not exist.</p>
      </div>
    </div>

    <div v-if="apiErrors.length">
      <ErrorsDisplay :errors="apiErrors"/>
    </div>
  </section>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router';
import { type  MovieListItem, useMovieStore } from '@/stores/movieStore';
import MovieDetails from '../components/MovieDetails.vue';
import ErrorsDisplay from '../components/ErrorsDisplay.vue';
import { storeToRefs } from 'pinia';

const route = useRoute();
const movieId = route.params.id;
const movieStore = useMovieStore();

movieStore.omdbQueryMovieById(movieId as string);
const {movieToDetail, isQuerying, apiErrors} = storeToRefs(movieStore);

const onAddMovieToFavouritesClick = (movie: MovieListItem) => {
  movieStore.addMovieToFavourites(movie);
};
const onRemoveMovieFromFavouritesClick = (movie: MovieListItem) => {
  movieStore.removeMovieFromFavourites(movie);
};
</script>

<style lang="scss" scoped>

.movie-page {
  .loading {
    .text {
      display: inline-flex;
      animation: growAndShrink 2.5s ease-in-out infinite;
    }
  }
}
</style>
