<script setup>
import { ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router';

// Reactive variable to manage mobile menu state
const isMobileMenuOpen = ref(false);

// Toggle function to change mobile menu state
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};
</script>

<template>
  <header class="bg-blue-500 text-white p-4">
    <nav class="flex justify-between items-center">
      <!-- Logo or Brand Name -->
      <div class="text-2xl font-bold">
        <RouterLink to="/">
          <div class="flex gap-2 items-center">
            <img src="../src/assets/images/movies.png" class="w-10 h-10 text-white" alt="Logo">
            Movies
          </div>
        </RouterLink>
      </div>

      <!-- Desktop Navigation Links -->
      <div class="hidden md:flex space-x-4">
        <RouterLink to="/" class="hover:text-yellow-300">Home</RouterLink>
        <RouterLink to="/about" class="hover:text-yellow-300">About</RouterLink>
      </div>

      <!-- Mobile Menu Button -->
      <div class="md:hidden">
        <button id="menu-toggle" class="focus:outline-none" @click="toggleMobileMenu">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
          </svg>
        </button>
      </div>
    </nav>

    <!-- Mobile Menu -->
    <div id="mobile-menu" class="md:hidden mt-4" :class="{'hidden': !isMobileMenuOpen, 'block': isMobileMenuOpen}">
      <RouterLink to="/" class="block hover:text-yellow-300">Home</RouterLink>
      <RouterLink to="/about" class="block hover:text-yellow-300">About</RouterLink>
    </div>
  </header>

  <main>
    <RouterView />
  </main>

  <footer class="bg-blue-500 text-white text-center p-4 mt-4">
    <div>
      <h2 class="text-lg font-semibold">Victor Zacharia</h2>
      <p class="text-sm">Software Engineer</p>
      <p class="text-sm">
        Victor Zacharia is a seasoned software engineer specializing in building dynamic and user-friendly web applications using modern frameworks.
      </p>
      <p class="text-sm">Contact: <a href="mailto:victor@example.com" class="hover:text-yellow-300">0683870268</a></p>
      <p class="text-xs mt-2">copyright © {{ new Date().getFullYear() }} Victor Zacharia. All rights reserved.</p> <!-- Copyright statement -->
    </div>
  </footer>

</template>

<style scoped>
/* Add any additional styles here */
</style>
