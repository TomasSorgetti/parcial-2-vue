<script>
import { RouterLink } from 'vue-router'

export default {
  name: 'DetailView',
  inject: ['isLoading', 'setLoading', 'showConfirm', 'showAlert'],
  components: {
    RouterLink,
  },
  data() {
    return {
      palette: null,
      favorite: false,
    }
  },
  methods: {
    deletePalette(id) {
      this.showConfirm('¿Estás seguro de eliminar esta paleta?', () => {
        this.setLoading(true)
        setTimeout(() => {
          // si la paleta es favorita la elimina
          this.removeFavorite()

          const palettes = JSON.parse(localStorage.getItem('palettes'))
          const newPalettes = palettes.filter((palette) => palette.id !== id)
          localStorage.setItem('palettes', JSON.stringify(newPalettes))
          this.setLoading(false)
          this.showAlert('Paleta eliminada exitosamente')
          this.$router.push('/')
        }, 500)
      })
    },
    addFavorite() {
      const favorites = JSON.parse(localStorage.getItem('favorites')) || []
      favorites.push(this.palette)
      localStorage.setItem('favorites', JSON.stringify(favorites))
      this.favorite = true
    },
    removeFavorite() {
      const favorites = JSON.parse(localStorage.getItem('favorites')) || []
      const newFavorites = favorites.filter((favorite) => favorite.id !== this.palette.id)
      localStorage.setItem('favorites', JSON.stringify(newFavorites))
      this.favorite = false
    },
  },
  mounted() {
    const id = this.$route.params.id
    const palettes = JSON.parse(localStorage.getItem('palettes'))
    this.palette = palettes.find((palette) => palette.id === parseInt(id)) || null

    const favorites = JSON.parse(localStorage.getItem('favorites'))
    if (!favorites) return
    this.favorite = favorites.some((favorite) => favorite.id === parseInt(id))
  },
}
</script>

<template>
  <div class="detail">
    <div v-if="!palette" class="detail-error">
      <p>No se encontró la paleta. Por favor, intentalo de nuevo.</p>
      <RouterLink to="/">Volver al inicio</RouterLink>
    </div>
    <div v-else class="detail-wrapper">
      <!-- Palette colors -->
      <div>
        <!-- <ul class="color-list">
          <li v-bind:key="color" v-for="color in palette.colors" class="color-circle">
            <span>{{ color }}</span>
          </li>
        </ul> -->
      </div>
      <!-- Palette info -->
      <div>
        <h1>{{ palette.name }}</h1>
        <p>{{ palette.description }}</p>
        <div class="palette-info">
          <div class="mode-info">
            <div class="mode-circle"></div>
            <p>{{ palette.mode }}</p>
          </div>
          <div class="category-info">
            <div class="category-circle"></div>
            <p>{{ palette.category }}</p>
          </div>
        </div>

        <button v-if="!favorite" @click="addFavorite()">Añadir a favoritos</button>
        <button v-else @click="removeFavorite()">Eliminar de favoritos</button>
        <button class="delete-palette" @click="deletePalette(palette.id)">Eliminar paleta</button>
      </div>
    </div>
  </div>
</template>

<style>
.detail {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;

  .detail-error {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    a {
      color: var(--light-text-color);
    }
  }

  h1 {
    text-transform: uppercase;
  }
  .palette-info {
    display: flex;
    gap: 1rem;
    .mode-info {
      display: flex;
      gap: 0.5rem;
      align-items: center;
    }
    .category-info {
      display: flex;
      gap: 0.5rem;
      align-items: center;
    }
    .mode-circle {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background-color: var(--primary-color);
    }
    .category-circle {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background-color: rgb(0, 183, 255);
    }
  }

  .delete-palette {
    color: red;
    background-color: transparent;
    border: none;
    display: block;
    cursor: pointer;
  }
}
</style>
