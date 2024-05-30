<template>
  <div class="bg-gray-900 min-h-screen">
    <!-- Navigation Bar -->
    <nav class="absolute top-0 w-full bg-black text-white p-4 flex justify-between items-center z-10">
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
      </nav> <br> <br> 
    <div class="container mx-auto text-center">
      <h1 class="text-4xl font-bold text-yellow-300 my-8">Rick and Morty Locations</h1>
      <!-- Loading or error message -->
      <div v-if="loading" class="text-center text-gray-600">Loading...</div>
      <div v-else-if="error" class="text-center text-red-600">{{ error.message }}</div>
      <div v-else>
        <!-- Locations display -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div v-for="(loc, index) in displayedLocations" :key="index" class="bg-gray-800 shadow-md  overflow-hidden transform transition-transform hover:bg-gray-500 flex flex-col items-center">
            <!-- Location information -->
            <div class="p-4 flex flex-col items-center justify-center text-white">
              <h2 class="text-xl font-semibold text-yellow-300 mb-2">{{ loc.name }}</h2>
              <p><b>Type:</b> {{ loc.type }}</p>
              <p><b>Dimension:</b> {{ loc.dimension }}</p>
              <p><b>Created:</b> {{ loc.created }}</p>
            </div>
            <!-- Residents -->
            <div class="flex flex-wrap justify-center p-4">
              <div v-for="(resident, i) in (expandedResidents[index] ? loc.residents : loc.residents.slice(0, 3))" :key="i" class="flex flex-col items-center  p-4 m-2">
                <img :src="resident.image" :alt="resident.name" class="w-16 h-16 object-cover mb-2">
                <div class="text-sm text-center text-white">
                  <p class="mb-1"><b>Name:</b> {{ resident.name }}</p>
                  <p class="mb-1"><b>Status:</b> {{ resident.status }}</p>
                  <p class="mb-1"><b>Species:</b> {{ resident.species }}</p>
                  <p class="mb-1"><b>Gender:</b> {{ resident.gender }}</p>
                </div>
              </div>
            </div>
            <!-- See More button -->
            <div v-if="loc.residents.length > 3" class="flex justify-center w-full mt-4">
              <button @click="toggleExpanded(index)" class="flex items-center justify-center w-8 h-8 bg-yellow-300 text-white rounded-full  transition">
                <span v-if="expandedResidents[index]">-</span>
                <span v-else>+</span>
              </button>
            </div>
          </div>
        </div>
        <!-- Show More and Show Less buttons -->
        <div class="text-center mt-4" v-if="showMoreButton || showLessButton">
          <button v-if="showMoreButton" @click="showMore" class="bg-yellow-300 text-black px-4 py-2 rounded hover:bg-yellow-300 transition">Show More</button>
          <button v-if="showLessButton" @click="showLess" class="bg-yellow-300 text-black px-4 py-2 rounded hover:bg-yellow-300 transition">Show Less</button>
        </div>
      </div>
      <footer class="bg-gray-900 text-white text-center py-4 w-full">
       <div class="flex justify-center items-center space-x-4">
        <a href="https://www.figma.com/design/3GaJDf7aBItuQXQR8frLN7/Untitled?node-id=0-1&t=WHWyfXHN0MDJBnXy-1" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">Click here to see my Figma Design</a>
        <a href="https://github.com/arsemada" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">GitHub</a> 
        </div> <br><p class="text-sm">&copy; 2024 Rick and Morty. All rights reserved.</p>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

const LOCATION_QUERY = gql`
  query Location {
    locations {
      results {
        name
        type
        dimension
        created
        residents {
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

const { result, loading, error } = useQuery(LOCATION_QUERY);

const initialLocationsCount = 10
const locationsToShow = ref(initialLocationsCount)

const displayedLocations = computed(() => {
  return result.value?.locations.results.slice(0, locationsToShow.value) || []
})

const showMoreButton = computed(() => {
  return result.value && locationsToShow.value < result.value.locations.results.length
})

const showLessButton = computed(() => {
  return locationsToShow.value > initialLocationsCount
})

const expandedResidents = ref([]);

const toggleExpanded = (index) => {
  expandedResidents.value[index] = !expandedResidents.value[index];
}

function showMore() {
  locationsToShow.value += initialLocationsCount;
}

function showLess() {
  locationsToShow.value = initialLocationsCount;
}
</script>

<style scoped>
/* Additional styles for the residents' containers */
.bg-gray-800 {
  background-color: #1f2937; /* Dark gray */
  color: #f3f4f6; /* Light gray */
  border-radius: 1rem; /* Rounded corners */
  border: 1px solid #374151; /* Border color */
  transition: transform 0.3s ease; /* Smooth transition */
}

.bg-gray-800:hover {
  background-color: #374151;
  /* Slightly enlarge on hover */
}
</style>