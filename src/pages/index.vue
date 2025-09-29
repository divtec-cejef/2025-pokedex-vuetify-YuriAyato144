<template>
  <v-container>
    <h1 class="mb-6 text-center">Pokédex</h1>

    <!-- Barre de recherche -->
    <v-text-field
      v-model="search"
      label="Rechercher un Pokémon"
      prepend-icon="mdi-magnify"
      clearable
      class="mb-6"
    />

    <v-row>
      <!-- Aucun Pokémon trouvé -->
      <v-col v-if="filteredPokemons.length === 0" cols="12">
        <v-alert type="warning">Aucun Pokémon trouvé.</v-alert>
      </v-col>

      <!-- Liste des Pokémons -->
      <v-col
        v-for="pokemon in filteredPokemons"
        :key="pokemon.id"
        cols="12"
        sm="6"
        md="4"
        lg="3"
        xl="2"
      >
        <v-card class="mb-4 pa-4">
          <v-card-title class="justify-space-between">
            {{ pokemon.name }}
            <v-btn
              icon
              :color="pokemonStore.isFavorite(pokemon) ? 'red' : 'grey'"
              @click="toggleFavori(pokemon)"
            >
              <v-icon>{{ pokemonStore.isFavorite(pokemon) ? 'mdi-heart' : 'mdi-heart-outline' }}</v-icon>
            </v-btn>
          </v-card-title>

          <v-card-text>
            <div>ID: {{ pokemon.id }}</div>
            <div>Type: {{ pokemon.type }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { usePokemonStore } from '@/stores/pokemonStore'

// Store Pinia
const pokemonStore = usePokemonStore()

// Barre de recherche
const search = ref('')

// Filtrer les Pokémons selon la recherche
const filteredPokemons = computed(() => {
  const query = search.value.toLowerCase().trim()
  return pokemonStore.pokemons.filter(pokemon =>
    pokemon.name.toLowerCase().includes(query)
  )
})

// Ajouter / retirer des favoris
function toggleFavori(pokemon) {
  pokemonStore.toggleFavorite(pokemon)
}

// Charger les données au montage
onMounted(async () => {
  if (pokemonStore.pokemons.length === 0) {
    await pokemonStore.init()
  }
  console.log('Pokémons chargés:', pokemonStore.pokemons.length)
})
const sortedPokemons = computed(() => {
  return [...pokemonStore.pokemons].sort((a, b) =>
    a.name.localeCompare(b.name)
  )
})
</script>
