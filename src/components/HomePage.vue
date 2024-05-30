<template>
  <div class="bg-gray-900 min-h-screen">
    <!-- Header Section with Carousel and Navigation Bar -->
    <header class="relative w-full h-screen">
      <!-- Carousel Section -->
      <div class="absolute inset-0 w-full h-full z-0">
        <div class="relative w-full h-full">
          <div v-for="(image, index) in images" :key="index" :class="['absolute inset-0 transition-opacity duration-700 ease-in-out', { 'opacity-0': index !== currentIndex, 'opacity-100': index === currentIndex }]" class="carousel-item">
            <img :src="image" class="w-full h-full object-cover p-20" alt="Rick and Morty" />
          </div>
        </div>
      </div>

      <!-- Navigation Bar -->
      <nav class="absolute top-0 w-full bg-transparent text-white p-4 flex justify-between items-center z-10">
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
      </nav>

      <!-- Navigation Dots -->
      <div class="absolute bottom-4 left-1/2 transform -translate-x-1/2 flex space-x-2 z-10">
        <button v-for="(image, index) in images" :key="index" @click="currentIndex = index" :class="['w-3 h-3 rounded-full', { 'bg-gray-700': index !== currentIndex, 'bg-yellow-500': index === currentIndex }]"></button>
      </div>
    </header>

    <!-- Description and Details -->
    <div class="container mx-auto py-10 px-4">
  <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">
    <!-- Description Section -->
    <div class="max-w-lg mx-auto lg:mx-0">
      <div class="bg-gray-900 text-white rounded-lg shadow-lg p-8">
        <h2 class="text-3xl font-bold mb-4">About "Rick and Morty"</h2>
        <p class="text-lg leading-relaxed">
          "Rick and Morty" is an animated science fiction sitcom that has gained a massive following since its debut in 2013. Created by Justin Roiland and Dan Harmon, the show follows the misadventures of an alcoholic and eccentric scientist named Rick Sanchez and his good-hearted but easily influenced grandson, Morty Smith. The series takes a unique approach to storytelling, blending elements of science fiction, adventure, and dark comedy to create a truly unique viewing experience. With its memorable characters, intricate plotlines, and thought-provoking themes, "Rick and Morty" has become a cultural phenomenon and a favorite among fans of animated television.
        </p>
      </div>
    </div>
    
    <!-- Details Section -->
    <div>
      <div class="grid grid-cols-2 gap-6 lg:grid-cols-1">
        <div class="bg-gray-900 text-white rounded-lg shadow-lg p-6 flex items-center">
          <span class="text-3xl mr-4">🎬</span>
          <div>
            <h3 class="text-xl font-semibold mb-2">Genre</h3>
            <p class="text-lg">Animated Science Fiction</p>
          </div>
        </div>
        <div class="bg-gray-900 text-white rounded-lg shadow-lg p-6 flex items-center">
          <span class="text-3xl mr-4">👨‍🔬</span>
          <div>
            <h3 class="text-xl font-semibold mb-2">Creators</h3>
            <p class="text-lg">Dan Harmon, Justin Roiland</p>
          </div>
        </div>
        <div class="bg-gray-900 text-white rounded-lg shadow-lg p-6 flex items-center">
          <span class="text-3xl mr-4">⭐️</span>
          <div>
            <h3 class="text-xl font-semibold mb-2">Stars</h3>
            <p class="text-lg">4.5/5</p>
          </div>
        </div>
        <div class="bg-gray-900 text-white rounded-lg shadow-lg p-6 flex items-center">
          <span class="text-3xl mr-4">🌟</span>
          <div>
            <h3 class="text-xl font-semibold mb-2">IMDb Rating</h3>
            <p class="text-lg">9.2/10</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>


    <div class="container mx-auto p-4">
      <p v-if="error" class="text-red-500 text-lg">Something went wrong...</p>
      <p v-if="loading" class="text-yellow-300 text-lg">Loading...</p>
      <div v-else class="space-y-6">
        <!-- Episodes Section -->
        <div>
          <h2 class="text-3xl font-bold text-yellow-300 mb-6">Episodes</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="ep in displayedEpisodes" :key="ep.id" class="bg-gray-800 shadow-md rounded-lg p-4 hover:bg-gray-700 transition duration-300 ease-in-out" @click="navigateToEpisode(ep.id)">
              <h3 class="font-semibold text-xl text-white">{{ ep.name }}</h3>
              <p class="text-white">{{ ep.episode }}</p>
              <p class="text-white">{{ ep.air_date }}</p>
              <p class="text-white">{{ ep.created }}</p>
            </div>
          </div>
          <button v-if="displayedEpisodes.length < allEpisodes.length" @click="showMoreEpisodes" class="bg-yellow-400 hover:bg-yellow-500 text-gray-900 mt-4 py-2 px-4 rounded">Show More</button>
        </div>
        <!-- Characters Section -->
        <div>
          <h2 class="text-3xl font-bold text-yellow-300 mb-6">Characters</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="character in displayedCharacters" :key="character.id" class="bg-gray-800 shadow-md rounded-lg p-4 hover:bg-gray-700 transition duration-300 ease-in-out">
              <div class="flex flex-col items-center">
                <img :src="character.image" alt="Character Image" class="w-32 h-32 object-cover rounded-full mb-4" @click="navigateToCharacter(character.id)">
                <h3 class="font-semibold text-xl text-white">{{ character.name }}</h3>
                <p class="text-white">{{ character.status }}</p>
                <p class="text-white">{{ character.species }}</p>
                <p class="text-white">{{ character.gender }}</p>
              </div>
            </div>
          </div>
          <button v-if="displayedCharacters.length < allCharacters.length" @click="showMoreCharacters" class="bg-yellow-400 hover:bg-yellow-500 text-gray-900 mt-4 py-2 px-4 rounded">Show More</button>
        </div>
        <!-- Locations Section -->
        <div>
          <h2 class="text-3xl font-bold  text-yellow-300 mb-6">Locations</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="location in displayedLocations" :key="location.id" class="bg-gray-800 shadow-md rounded-lg p-4 hover:bg-gray-700 transition duration-300 ease-in-out" @click="navigateToLocation(location.id)">
              <h3 class="font-semibold text-xl text-white">{{ location.name }}</h3>
            </div>
          </div>
          <button v-if="displayedLocations.length < allLocations.length" @click="showMoreLocations" class="bg-yellow-400 hover:bg-yellow-500 text-gray-900 mt-4 py-2 px-4 rounded">Show More</button>
        </div>
      </div>
    </div>
    <footer class="bg-gray-900 text-white text-center py-4 w-full">
       <div class="flex justify-center items-center space-x-4">
        <a href="https://www.figma.com/design/3GaJDf7aBItuQXQR8frLN7/Untitled?node-id=0-1&t=WHWyfXHN0MDJBnXy-1" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">Click here to see my Figma Design</a>
        <a href="https://github.com/arsemada" target="_blank" rel="noopener noreferrer" class=" hover:text-yellow-300 transition-colors">GitHub</a> 
        </div> <br><p class="text-sm">&copy; 2024 Rick and Morty. All rights reserved.</p>
      </footer>

  </div>
</template>

<script setup>
import { ref, onMounted, computed, watchEffect } from 'vue';
import { useRouter } from 'vue-router';
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable';

const router = useRouter();

const navigateToEpisode = (id) => {
  router.push({ name: 'EpisodeDetails', params: { id } });
};

const navigateToCharacter = (id) => {
  router.push({ name: 'CharacterDetails', params: { id } });
};

const navigateToLocation = (id) => {
  router.push({ name: 'LocationDetails', params: { id } });
};

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
          id
          name
        }
        episode {
          id
          name
          episode
          air_date
          created
        }
      }
    }
  }
`;

const { result, loading, error } = useQuery(CHARACTERS_QUERY);

const getUniqueItems = (items, key) => {
  const uniqueKeys = new Set();
  return items.filter(item => {
    if (!uniqueKeys.has(item[key])) {
      uniqueKeys.add(item[key]);
      return true;
    }
    return false;
  });
};

const uniqueEpisodes = computed(() => {
  const episodes = result.value ? result.value.characters.results.flatMap(character => character.episode) : [];
  return getUniqueItems(episodes, 'id');
});

const uniqueCharacters = computed(() => result.value ? getUniqueItems(result.value.characters.results, 'id') : []);

const uniqueLocations = computed(() => {
  const locations = result.value ? result.value.characters.results.map(character => character.location).filter(location => location !== null) : [];
  return getUniqueItems(locations, 'id');
});

const allEpisodes = ref([]);
const allCharacters = ref([]);
const allLocations = ref([]);
const displayedEpisodes = ref([]);
const displayedCharacters = ref([]);
const displayedLocations = ref([]);

watchEffect(() => {
  if (uniqueEpisodes.value.length) {
    allEpisodes.value = uniqueEpisodes.value;
    displayedEpisodes.value = allEpisodes.value.slice(0, 6);
  }
  if (uniqueCharacters.value.length) {
    allCharacters.value = uniqueCharacters.value;
    displayedCharacters.value = allCharacters.value.slice(0, 6);
  }
  if (uniqueLocations.value.length) {
    allLocations.value = uniqueLocations.value;
    displayedLocations.value = allLocations.value.slice(0, 6);
  }
});

const showMoreEpisodes = () => {
  const startIndex = displayedEpisodes.value.length;
  const endIndex = Math.min(startIndex + 6, allEpisodes.value.length);
  displayedEpisodes.value = [...displayedEpisodes.value, ...allEpisodes.value.slice(startIndex, endIndex)];
};

const showMoreCharacters = () => {
  const startIndex = displayedCharacters.value.length;
  const endIndex = Math.min(startIndex + 6, allCharacters.value.length);
  displayedCharacters.value = [...displayedCharacters.value, ...allCharacters.value.slice(startIndex, endIndex)];
};

const showMoreLocations = () => {
  const startIndex = displayedLocations.value.length;
  const endIndex = Math.min(startIndex + 6, allLocations.value.length);
  displayedLocations.value = [...displayedLocations.value, ...allLocations.value.slice(startIndex, endIndex)];
};

const images = [
  'https://images.app.goo.gl/46PdWVe5jq6kKBKYA',
  'https://images.app.goo.gl/tnGtXafTKrrmiokz9',
  'https://images.app.goo.gl/Atr3mtH4C7kD9iir8',
  'https://images.app.goo.gl/ffwanHMm3BLC21m56',
  'https://images.app.goo.gl/CSF9G7h8Pbvmkjzg8',
  'https://images.app.goo.gl/aZGVENepRWuvhEUz9',

];

const currentIndex = ref(0);
const currentImage = computed(() => images[currentIndex.value]);

const next = () => {
  currentIndex.value = (currentIndex.value + 1) % images.length;
};

const prev = () => {
  currentIndex.value = (currentIndex.value - 1 + images.length) % images.length;
};

onMounted(() => {
  setInterval(next, 4000);
});
</script>

<style>
.carousel-item {
  transition: opacity 0.5s ease-in-out;
}
</style>
