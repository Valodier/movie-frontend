<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Movies!",
      movies: {},
      newMovieParams: {},
    };
  },
  created: function () {
    this.movieIndex();
  },
  methods: {
    movieIndex: function () {
      axios.get("/movies.json").then((response) => {
        this.movies = response.data;
        console.log("Movies index", response);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="movie in movies" :key="movie.id">
      <h1>{{ movie.title }}</h1>
    </div>
  </div>
</template>

<style></style>
