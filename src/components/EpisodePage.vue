<template>
  <div class="bg-gray-900 min-h-screen">
    <!-- Navigation Bar -->
    <nav class="fixed top-0 w-full bg-black text-white p-4 flex justify-between items-center z-10">
      <div class="text-2xl font-bold">
        <router-link to="/" class="hover:text-yellow-300">Rick and Morty</router-link>
      </div>
      <ul class="flex">
        <li class="mx-4">
          <router-link to="/" class="hover:text-yellow-300">Home</router-link>
        </li>
        <li class="mx-4">
          <router-link to="/Episodes" class="hover:text-yellow-300">Episodes</router-link>
        </li>
        <li class="mx-4">
          <router-link to="/characters" class="hover:text-yellow-300">Characters</router-link>
        </li>
        <li class="mx-4">
          <router-link to="/Locations" class="hover:text-yellow-300">Locations</router-link>
        </li>
      </ul>
      <button class="hover:text-yellow-300">Login</button>
    </nav> <br> <br> <br> <br>
    <div class="container mx-auto bg-gray-900 p-2">
      <!--<h1 class="text-3xl font-bold text-center text-yellow-300 my-4">Rick and Morty Episodes</h1>-->
      <ul>
        <p v-if="error" class="text-red-500 text-lg">Something went wrong...</p>
        <p v-if="loading" class="text-yellow-300 text-lg">Loading...</p>
        <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <div v-for="(ep, index) in result.episodes.results" :key="ep.id" class="bg-gradient-to-br from-gray-800 to-gray-900 shadow-lg rounded-lg overflow-hidden hover:shadow-xl transition duration-300 ease-in-out">
            <div class="px-6 py-4">
              <h2 class="text-lg font-bold text-yellow-300">{{ ep.name }}</h2>
              <p class="text-gray-300 text-sm mb-2"><strong>Episode Code:</strong> {{ ep.episode }}</p>
              <p class="text-gray-300 text-sm mb-2"><strong>Air Date:</strong> {{ ep.air_date }}</p>
            </div>
            <div class="px-6 pb-4">
              <div class="flex flex-wrap -m-2">
                <div v-for="(character, i) in ep.characters.slice(0, 4)" :key="i" class="m-2">
                  <img :src="character.image" :alt="character.name" class="w-12 h-12 rounded-full border-2 border-gray-300 shadow-md">
                  <p class="text-white font-semibold text-sm mt-2">{{ character.name }}</p>
                </div>
              </div>
              <!-- See More Button -->
              <button @click="showMore(index)" class="mt-4 bg-yellow-400 text-gray-900 text-xs font-semibold rounded-md py-1 px-2 hover:bg-yellow-500 transition duration-300 ease-in-out inline-flex items-center">
              <span v-if="expandedIndex !== index">Show More</span>
              <span v-else>Show Less</span>
              </button>




              <!-- Additional characters displayed when See More button is clicked -->
              <div v-if="expandedIndex === index" class="mt-4">
                <div class="flex flex-wrap -m-2">
                  <div v-for="(character, i) in ep.characters.slice(4)" :key="i" class="m-2">
                    <img :src="character.image" :alt="character.name" class="w-12 h-12 rounded-full border-2 border-gray-300 shadow-md">
                    <p class="text-white font-semibold text-sm mt-2">{{ character.name }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </ul>
    </div>

  </div>
  <footer class="bg-gray-900 text-white text-center py-4 w-full">
       <div class="flex justify-center items-center space-x-4">
        <a href="https://www.figma.com/design/3GaJDf7aBItuQXQR8frLN7/Untitled?node-id=0-1&t=WHWyfXHN0MDJBnXy-1" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">Click here to see my Figma Design</a>
        <a href="https://github.com/arsemada" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">GitHub</a> 
        </div> <br><p class="text-sm">&copy; 2024 Rick and Morty. All rights reserved.</p>
      </footer>

</template>

<script setup>
import { ref } from 'vue'
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

const expandedIndex = ref(null)

const Episode_QUERY = gql`
  query Episode {
    episodes {
      results {
        id
        name
        episode
        air_date
        created
        characters {
          name
          status
          species
          gender
          image
        }
      }
    }
  }
`

const { result, loading, error } = useQuery(Episode_QUERY);

function showMore(index) {
  expandedIndex.value = expandedIndex.value === index ? null : index
}
</script>

<style scoped>
/* Tailwind CSS import */
@import 'https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css';

/* Adjust spacing for better presentation */
ul {
  margin: 0;
  padding: 0;
}

/* Adjust margin around container */
.container {
  margin: 0.5rem auto;
}

/* Style for episode container */
.bg-gradient-to-br {
  background: linear-gradient(45deg, #4a5568, #2d3748); /* Gradient from gray-800 to gray-900 */
}

/* Hover effect for episode container */
.bg-gradient-to-br:hover {
  transform: translateY(-4px); /* Lift on hover */
}

/* Hover effect for see more button */
button:hover {
  background-color: #d97706; /* Darker yellow on hover */
}
</style>
