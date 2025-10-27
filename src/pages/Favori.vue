<template>
  <v-container>
    <h1 class="mb-6 text-center">Mes Pokémons Favoris</h1>

    <!-- Liste des favoris -->
    <v-row v-if="favoris.length > 0">
      <v-col
        v-for="pokemon in favoris"
        :key="pokemon.id"
        cols="12"
        lg="3"
        md="4"
        sm="6"
        xl="2"
      >
        <PokemonCard :pokemon="pokemon" />
      </v-col>
    </v-row>

    <!-- Message si aucun favori -->
    <v-alert
      v-else
      class="text-center"
      prominent
      type="info"
    >
      <div class="mb-4">
        <v-icon color="info" size="64">mdi-heart-outline</v-icon>
      </div>
      <div class="text-h6 mb-2">Aucun Pokémon favori</div>
      <div class="mb-4">
        Vous n'avez pas encore ajouté de Pokémon à vos favoris.<br>
        Cliquez sur le cœur d'un Pokémon pour l'ajouter ici !
      </div>
      <v-btn
        color="primary"
        prepend-icon="mdi-pokeball"
        to="/"
      >
        Retourner à la liste
      </v-btn>
    </v-alert>
  </v-container>
</template>

<script setup>
  import { computed } from 'vue'
  import { usePokemonStore } from '@/stores/pokemonStore'
  import PokemonCard from '@/components/PokemonCard.vue'

  const pokemonStore = usePokemonStore()

  // Récupérer les Pokémons favoris
  const favoris = computed(() => pokemonStore.getFavorites)
</script>
