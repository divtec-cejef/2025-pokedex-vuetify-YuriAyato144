<template>
  <v-card
    class="pokemon-card"
    elevation="2"
    hover
  >
    <!-- Image du Pokémon -->
    <v-img
      class="pokemon-image"
      contain
      height="200"
      :src="`/images/${pokemon.img}`"
    />

    <!-- Bouton favori (cœur) -->
    <v-btn
      class="favorite-btn"
      icon
      @click.prevent="pokemonStore.toggleFavorite(pokemon)"
    >
      <v-icon :color="pokemonStore.isFavorite(pokemon) ? 'red' : 'grey'">
        {{ pokemonStore.isFavorite(pokemon) ? 'mdi-heart' : 'mdi-heart-outline' }}
      </v-icon>
    </v-btn>

    <!-- Informations du Pokémon -->
    <v-card-title class="text-center">
      {{ pokemon.name }}
    </v-card-title>

    <v-card-subtitle class="text-center">
      Niveau {{ pokemon.level }}
    </v-card-subtitle>

    <v-card-text>
      <!-- Affichage des types -->
      <div class="d-flex justify-center flex-wrap ga-2">
        <v-chip
          v-for="typeId in (Array.isArray(pokemon.type) ? pokemon.type : [])"
          :key="typeId"
          :color="getTypeColor(typeId)"
          size="small"
          text-color="white"
        >
          {{ getTypeName(typeId) }}
        </v-chip>
      </div>
    </v-card-text>
  </v-card>
</template>

<script setup>
  import { usePokemonStore } from '@/stores/pokemonStore'

  // Props
  defineProps({
    pokemon: {
      type: Object,
      required: true,
    },
  })

  // Store
  const pokemonStore = usePokemonStore()
</script>

<style scoped>
.pokemon-card {
  position: relative;
  transition: transform 0.2s;
}

.pokemon-card:hover {
  transform: translateY(-4px);
}

.pokemon-image {
  background: linear-gradient(to bottom, #f5f5f5, #ffffff);
}

.favorite-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  z-index: 1;
  background-color: rgba(255, 255, 255, 0.9);
}
</style>
