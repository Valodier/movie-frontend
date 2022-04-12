<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Movies!",
      movies: {},
      newMovieParams: {},
      currentMovie: {},
      editMovieParams: {},
    };
  },
  created: function () {
    this.movieIndex();
  },
  methods: {
    movieIndex: function () {
      axios.get("/movies.json").then((response) => {
        this.movies = response.data;
        console.log("Movies index", response.data);
      });
    },
    movieCreate: function () {
      axios
        .post("/movies.json", this.newMovieParams)
        .then((response) => {
          console.log("Created Successfully!", response);
          this.movies.push(response.data);
          this.newMovieParams = {};
        })
        .catch((error) => {
          console.log("Error creating movie", error.response.data);
        });
    },
    movieShow: function (movie) {
      this.currentMovie = movie;
      this.editMovieParams = movie;
      document.querySelector("#movie-details").showModal();
    },
    movieUpdate: function (movie) {
      axios
        .patch("/movies/" + movie.id + ".json", this.editMovieParams)
        .then((response) => {
          console.log("Movie updated", response.data);
          this.currentMovie = {};
        });
    },
    movieDestroy: function (movie) {
      axios.patch("/movies/" + movie.id + ".json").then((response) => {
        console.log("movie destroyed", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <div>
      <h1>New Movie</h1>
      Title:
      <input type="text" v-model="newMovieParams.title" />
      Director:
      <input type="text" v-model="newMovieParams.director" />
      Actors:
      <input type="text" v-model="newMovieParams.actors" />
      Plot:
      <input type="text" v-model="newMovieParams.plot" />
      Genre:
      <input type="text" v-model="newMovieParams.genre" />
      Year:
      <input type="text" v-model="newMovieParams.year" />
      <button v-on:click="movieCreate()">Make your own Movie!</button>
    </div>
    <h1>Show me the movies!</h1>
    <div v-for="movie in movies" :key="movie.id">
      <h1>{{ movie.title }}</h1>
      <h2>{{ movie.id }}</h2>
      <p>{{ movie.plot }}</p>
      <button v-on:click="movieShow(movie)">The Deets</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h1>The Deets</h1>
        Director:
        <input type="text" v-model="editMovieParams.director" />
        <p>Plot: {{ currentMovie.plot }}</p>
        <input type="text" v-model="editMovieParams.plot" />
        <p>Year: {{ currentMovie.year }}</p>
        <input type="text" v-model="editMovieParams.year" />
        <button v-on:click="movieUpdate(currentMovie)">Update</button>
        <button v-on:click="movieDestroy(currentMovie)">DESTROY</button>
        <button>Bye Bye</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
