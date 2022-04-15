<template>
  <div>
    <h1>{{ movie.title }}</h1>
    <p>{{ movie.plot }}</p>
    <router-link v-bind:to="`/movies/${movie.id}/edit`">
      Edit Movie
    </router-link>
    |
    <router-link to="/movies"> Back to All Movies </router-link>
    <br />
    <button v-on:click="movieDestroy(movie)">DESTROY</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get("/movies/" + this.$route.params.id + ".json").then((response) => {
      console.log("Movies show", response);
      this.movie = response.data;
    });
  },
  methods: {
    movieDestroy: function (movie) {
      axios.delete("/movies/" + movie.id + ".json").then((response) => {
        console.log("Forever gone", response.data);
        this.$router.push("/movies");
      });
    },
  },
};
</script>
