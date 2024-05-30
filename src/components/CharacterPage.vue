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
    </nav> <br> <br>

    <div class="container mx-auto mt-16">
      <h1 class="text-3xl font-bold text-center text-yellow-300 mb-8">Meet the Characters</h1>

      <div class="container mx-auto p-4 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div v-for="character in result.characters.results" :key="character.id" class="bg-gray-800 shadow-md rounded-lg overflow-hidden transform transition-transform hover:scale-105 flex flex-col">
          <img :src="character.image" alt="Character Image" class="w-full h-64 object-cover rounded-t-lg">
          <div class="p-4">
            <h3 class="font-semibold text-lg text-yellow-300 mb-2">{{ character.name }}</h3>
            <p class="text-sm text-gray-400 mb-2">{{ character.status }} | {{ character.species }} | {{ character.gender }}</p>
            <div class="mt-4">
              <h4 class="text-sm font-semibold mb-2 text-gray-400">Location</h4>
              <p class="text-sm text-gray-300">{{ character.location ? character.location.name : 'Unknown' }}</p>
            </div>
            <!-- Show More Button for Episodes -->
            <button @click="toggleEpisodes(character.id)" class="text-sm text-gray-400 hover:text-yellow-300 focus:outline-none mt-4">
              {{ isEpisodesVisible(character.id) ? 'Show Less' : 'Show More Episodes' }}
            </button>
            <!-- Episodes (conditionally displayed) -->
            <div v-if="isEpisodesVisible(character.id)" class="mt-4">
              <h4 class="text-sm font-semibold mb-2 text-gray-400">Episodes</h4>
              <div class="grid grid-cols-1 gap-2">
                <div v-for="(ep, index) in character.episode" :key="ep.id" v-show="index < maxEpisodes" class="bg-gray-700 p-2 rounded-md text-xs text-gray-300">
                  <span>{{ ep.name }} ({{ ep.episode }}) - {{ ep.air_date }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
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

const CHARACTERS_QUERY = gql`
  query Characters {
    characters {
      results {
        id
        name
        image
        status
        gender
        species
        location {
          name
        }
        episode {
          id
          name
          episode
          air_date
        }
      }
    }
  }
`
const { result, loading, error } = useQuery(CHARACTERS_QUERY);

// Object to track the visibility state of episodes for each character
const episodesVisibility = ref({});

// Maximum number of episodes to display initially
const maxEpisodes = 3;

// Function to toggle the visibility state of episodes for a character
const toggleEpisodes = (characterId) => {
  episodesVisibility.value[characterId] = !episodesVisibility.value[characterId];
};

// Function to check if episodes are visible for a character
const isEpisodesVisible = (characterId) => {
  return !!episodesVisibility.value[characterId];
};
</script>

<style scoped>
/* Add your scoped styles here */
</style>
