<script>
import PaletteCard from '../components/ui/PaletteCard.vue'

export default {
  name: 'FavoritesView',
  inject: ['isLoading', 'setLoading', 'showConfirm', 'showAlert'],
  components: {
    PaletteCard,
  },
  data() {
    return {
      favorites: [],
    }
  },
  mounted() {
    const storedFavorites = localStorage.getItem('favorites')
    if (storedFavorites) {
      this.favorites = JSON.parse(storedFavorites)
    }
  },
}
</script>

<template>
  <div class="favorites">
    <h1>Tus paletas favoritas</h1>
    <p v-if="favorites.length === 0">No tienes paletas favoritas. Crea una y agregala!</p>
    <div class="palette-list">
      <PaletteCard
        v-for="palette in favorites"
        :key="palette.id"
        :palette="palette"
        :isLoading="isLoading"
      />
    </div>
  </div>
</template>

<style>
.favorites {
  min-height: 80vh;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
  gap: 4rem;

  .palette-list {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    width: 100%;
  }
}
</style>
