<template>
  <div class="container mx-auto mt-8 p-4 bg-gray-900 text-white rounded-lg shadow-2xl">
    <div v-if="loading" class="text-yellow-300 text-lg font-semibold">Loading...</div>
    <div v-if="error" class="text-red-500 text-lg font-semibold">Something went wrong...</div>
    <div v-if="character">
      <h1 class="text-4xl font-bold text-yellow-300 mb-6">{{ character.name }}</h1>
      <div class="flex flex-col items-center sm:flex-row">
        <img :src="character.image" alt="Character Image" class="w-48 h-48 object-cover rounded-lg mb-4 sm:mr-6">
        <div>
          <p class="text-lg mb-2"><b>Status:</b> {{ character.status }}</p>
          <p class="text-lg mb-2"><b>Species:</b> {{ character.species }}</p>
          <p class="text-lg mb-2"><b>Gender:</b> {{ character.gender }}</p>
          <p class="text-lg mb-2"><b>Location:</b> {{ character.location.name }}</p>
        </div>
      </div>
      <p class="text-xl font-semibold mt-6 mb-2">Episodes:</p>
      <ul class="list-disc list-inside pl-4">
        <li v-for="ep in character.episode" :key="ep.id" class="text-lg">{{ ep.name }} ({{ ep.episode }})</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import { useRoute } from 'vue-router'
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

const route = useRoute()
const characterId = route.params.id

const CHARACTER_QUERY = gql`
  query Character($id: ID!) {
    character(id: $id) {
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
      }
    }
  }
`

const { result, loading, error } = useQuery(CHARACTER_QUERY, { id: characterId })
const character = ref(null)

watchEffect(() => {
  if (result.value) {
    character.value = result.value.character
  }
})
</script>

<style scoped>
.container {
  background-color: #1f2937; /* Dark gray background */
  color: #f9fafb; /* Light gray text */
}

.text-yellow-300 {
  color: #fbbf24; /* Maintain the yellow color */
}

.text-red-500 {
  color: #ef4444; /* Custom red */
}

.text-lg {
  font-size: 1.125rem; /* Adjusted text size */
}

.font-semibold {
  font-weight: 600; /* Semi-bold */
}

.font-bold {
  font-weight: 700; /* Bold */
}

.shadow-2xl {
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25); /* Deeper shadow */
}

.rounded-lg {
  border-radius: 0.5rem; /* Rounded corners */
}

.list-disc {
  list-style-type: disc; /* Disc list style */
}

.list-inside {
  list-style-position: inside; /* List style inside */
}

@media (min-width: 640px) {
  .flex-col {
    flex-direction: column;
  }
  .sm\\:flex-row {
    flex-direction: row;
  }
  .sm\\:mr-6 {
    margin-right: 1.5rem;
  }
}
</style>
