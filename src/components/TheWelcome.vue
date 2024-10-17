<script setup>
import { ref } from "vue";
import { RouterLink } from "vue-router";
import env from "@/env.js";

const searchInput = ref("");
const movies = ref([]);
const errorMessage = ref(""); // Store error messages
const loading = ref(false); // To handle loading state

// Function to search movies
const searchMovies = async () => {
  if (searchInput.value.trim() === "") {
    errorMessage.value = "Please enter a search term.";
    movies.value = []; // Clear previous results
    return;
  }

  const query = encodeURIComponent(searchInput.value);
  const apiUrl = `https://www.omdbapi.com/?apikey=${env.apikey}&s=${query}`;

  loading.value = true; // Set loading to true

  try {
    const response = await fetch(apiUrl);

    // Check if the response is OK
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();

    if (data.Response === "True") {
      movies.value = data.Search;
      errorMessage.value = ""; // Clear any previous error messages
    } else {
      movies.value = [];
      errorMessage.value = "No results found.";
    }
  } catch (error) {
    console.error("Error fetching data:", error);
    errorMessage.value = "Error fetching data. Please try again later.";
  } finally {
    loading.value = false; // Reset loading state
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

// Function to download movie details
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

    <!-- Loading Indicator -->
    <div v-if="loading" class="text-center text-gray-500 mt-4">Loading...</div>

    <!-- Error Message -->
    <div v-if="errorMessage" class="text-center text-red-500 mt-4">{{ errorMessage }}</div>

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
            <i class="fas fa-download"></i>
            Download Poster
          </button>

          <!-- Download Movie Details as Text -->
          <button
            @click="downloadMovieDetails(movie)"
            class="mt-2 px-4 py-2 bg-blue-500 text-white font-semibold rounded-md hover:bg-blue-600 transition duration-200">
            <i class="fas fa-download"></i>
            Download Details
          </button>
        </div>
      </div>
    </div>

    <!-- No Movies Found -->
    <div v-else-if="!loading && !errorMessage" class="text-center text-gray-500 mt-8">
      No movies found. Please try searching for something else.
    </div>
  </div>
</template>

<style scoped>
/* Add your styles here */
</style>
