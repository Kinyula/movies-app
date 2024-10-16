<template>
  <div class="about px-4 lg:px-8 py-12 bg-gray-100">
    <h1 class="text-3xl font-bold text-center mb-8">About Us</h1>
    
    <!-- Cards Container -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-6xl mx-auto">

      <!-- Movie Access Card -->
      <div class="card bg-white shadow-lg rounded-lg p-6">
  <img 
    src="../assets/images/download.png" 
    alt="Movies Access" 
    class="w-full h-96 object-cover rounded-md mb-4">
  <h2 class="text-2xl font-semibold mb-4">Easy Access to Movies</h2>
  <p class="text-gray-700 mb-6">
    Our Vue-based application is designed with one primary goal: 
    to provide users with fast and easy access to a wide selection of movies. 
    Whether you're searching for the latest blockbusters or classic films, 
    the intuitive search feature allows you to find and explore movie details effortlessly. 
    With real-time data fetching and a responsive interface, browsing for your favorite movies 
    has never been easier.
  </p>
</div>


      <!-- Developer Skill Set Card -->
      <div class="card bg-white shadow-lg rounded-lg p-6">
        <img 
          src="../assets/images/profile.jpg" 
          alt="Developer Skill Set" 
          class="w-full h-96 object-cover rounded-md mb-4">
        <h2 class="text-2xl font-semibold mb-4">Software engineer Victor Zacharia</h2>
        <p class="text-gray-700 mb-6">
          This application is proudly developed by <strong>Victor Zacharia</strong>, a seasoned software engineer with extensive experience in building dynamic and user-friendly web applications. 
          Victor specializes in using Vue.js, a modern JavaScript framework, to create efficient and scalable front-end solutions. 
          His expertise in JavaScript, component-based architecture, and responsive design ensures that every project not only meets, 
          but exceeds user expectations.
        </p>
      </div>
    </div>

  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
