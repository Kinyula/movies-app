<script setup>
import { ref } from "vue";
import { RouterLink } from "vue-router";
import env from "@/env.js";

const searchInput = ref("");
const movies = ref([]);

// Function to search movies
const searchMovies = () => {
  if (searchInput.value !== "") {
    const query = encodeURIComponent(searchInput.value);
    const apiUrl = `http://www.omdbapi.com/?apikey=${env.apikey}&s=${query}`;

    fetch(apiUrl)
      .then((response) => response.json())
      .then((data) => {
        if (data.Response === "True") {
          movies.value = data.Search;
        } else {
          console.error("No results found");
          movies.value = [];
        }
      })
      .catch((error) => console.error("Error fetching data:", error));
  } else {
    console.log("Please enter a search term.");
  }
};

// Function to download the movie poster
const downloadPoster = (posterUrl, title) => {
  const link = document.createElement("a");
  link.href = posterUrl;
  link.download = `${title}_poster.jpg`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};


const downloadMovieDetails = (movie) => {

  const movieDetails = `
    Title: ${movie.Title}
    Year: ${movie.Year}
    IMDB ID: ${movie.imdbID}
    Type: ${movie.Type}
  `;

  // Create a Blob with the plain text data
  const blob = new Blob([movieDetails], { type: "text/plain" });
  const url = URL.createObjectURL(blob);

  // Create an anchor element and trigger download
  const link = document.createElement("a");
  link.href = url;
  link.download = `${movie.Title}_details.txt`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<template>
  <div class="home bg-gray-100 py-8">
    <!-- Search Form -->
    <form
      @submit.prevent="searchMovies"
      class="flex items-center space-x-4 bg-gray-100 p-4 rounded-lg shadow-md mt-8">
      <input
        type="text"
        v-model="searchInput"
        placeholder="Search for a movie..."
        class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent" />
      <button
        type="submit"
        class="px-4 py-2 bg-blue-500 text-white font-semibold rounded-md hover:bg-blue-600 transition duration-200">
        Search
      </button>
    </form>

    <!-- Movie List -->
    <div
      v-if="movies.length > 0"
      class="movie-list grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 mt-8">
      <div
        v-for="movie in movies"
        :key="movie.imdbID"
        class="movie-card bg-white shadow-md rounded-lg p-4">
        <RouterLink :to="`/movie/${movie.imdbID}`">
          <img
            :src="
              movie.Poster !== 'N/A'
                ? movie.Poster
                : 'https://via.placeholder.com/300x450?text=No+Image+Available'
            "
            :alt="movie.Title"
            class="w-full h-auto rounded-md mb-4" />
          <h3 class="text-lg font-semibold text-gray-800">{{ movie.Title }}</h3>
          <p class="text-gray-600">Year: {{ movie.Year }}</p>
        </RouterLink>

        <div class="flex gap-2">
          <!-- Download Poster Button -->
          <button
            @click="downloadPoster(movie.Poster, movie.Title)"
            v-if="movie.Poster !== 'N/A'"
            class="mt-2 px-4 py-2 bg-green-500 text-white font-semibold rounded-md hover:bg-green-600 transition duration-200">
            Download Poster
          </button>

          <!-- Download Movie Details as JSON -->
          <button
            @click="downloadMovieDetails(movie)"
            class="mt-2 px-4 py-2 bg-blue-500 text-white font-semibold rounded-md hover:bg-blue-600 transition duration-200">
            Download Details
          </button>
        </div>
      </div>
    </div>

    <!-- No Movies Found -->
    <div v-else class="text-center text-gray-500 mt-8">
      No movies found. Please try searching for something else.
    </div>
  </div>
</template>
