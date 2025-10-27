<template>
  <v-container>
    <h1 class="mb-6 text-center">Pokédex</h1>

    <!-- Barre de recherche -->
    <v-text-field
      v-model="search"
      class="mb-6"
      clearable
      label="Rechercher un Pokémon"
      prepend-icon="mdi-magnify"
    />

    <!-- Indicateur de chargement -->
    <v-progress-linear
      v-if="pokemonStore.isLoading"
      class="mb-4"
      color="primary"
      indeterminate
    />

    <!-- Message si aucun Pokémon chargé -->
    <v-alert v-if="!pokemonStore.isLoading && pokemonStore.pokemons.length === 0" class="mb-4" type="warning">
      Aucun Pokémon disponible. Vérifiez que le serveur API est démarré.
    </v-alert>

    <v-row>
      <!-- Aucun Pokémon trouvé dans la recherche -->
      <v-col v-if="filteredPokemons.length === 0 && pokemonStore.pokemons.length > 0" cols="12">
        <v-alert class="text-center" type="info">
          Aucun Pokémon ne correspond à votre recherche.
        </v-alert>
      </v-col>

      <!-- Liste des Pokémons -->
      <v-col
        v-for="pokemon in filteredPokemons"
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
  </v-container>
</template>

<script setup>
  import { computed, onMounted, ref } from 'vue'
  import { usePokemonStore } from '@/stores/pokemonStore'
  import PokemonCard from '@/components/PokemonCard.vue'

  const pokemonStore = usePokemonStore()

  // Barre de recherche
  const search = ref('')

  // Tri alphabétique
  const sortedPokemons = computed(() => {
    if (!pokemonStore.pokemons || pokemonStore.pokemons.length === 0) return []
    return [...pokemonStore.pokemons].sort((a, b) =>
      a.name.localeCompare(b.name)
    )
  })

  // Filtrage avec recherche
  const filteredPokemons = computed(() => {
    const query = search.value.toLowerCase().trim()
    if (!query) return sortedPokemons.value

    return sortedPokemons.value.filter(pokemon =>
      pokemon.name.toLowerCase().includes(query)
    )
  })

  // Charger les données au montage du composant
  onMounted(async () => {
    console.log('🔄 Initialisation de la page Pokédex...')

    // Si les Pokémons ne sont pas encore chargés, initialiser le store
    if (pokemonStore.pokemons.length === 0) {
      try {
        await pokemonStore.init()
        console.log('✅ Pokémons chargés:', pokemonStore.pokemons.length)
      } catch (error) {
        console.error('❌ Erreur lors du chargement:', error)
      }
    } else {
      console.log('ℹ️ Pokémons déjà chargés:', pokemonStore.pokemons.length)
    }
  })
</script>
