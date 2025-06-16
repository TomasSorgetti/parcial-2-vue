<script>
import SelectedPalette from '@/components/sections/SelectedPalette.vue'
import { RouterLink } from 'vue-router'

export default {
  name: 'DetailView',
  inject: ['isLoading', 'setLoading', 'showConfirm', 'showAlert'],
  components: {
    RouterLink,
    SelectedPalette,
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
      this.showAlert('Paleta agregada a favoritos')
    },
    removeFavorite() {
      const favorites = JSON.parse(localStorage.getItem('favorites')) || []
      const newFavorites = favorites.filter((favorite) => favorite.id !== this.palette.id)
      localStorage.setItem('favorites', JSON.stringify(newFavorites))
      this.favorite = false
      this.showAlert('Paleta eliminada de favoritos')
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
      <SelectedPalette :selectedPalette="palette.colors" />
      <!-- Palette info -->
      <div class="detail-info">
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

        <button v-if="!favorite" @click="addFavorite()" class="add-favorite">
          <div class="favorite"></div>
          Añadir a favoritos
        </button>
        <button v-else @click="removeFavorite()" class="remove-favorite">
          Eliminar de favoritos
        </button>
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

  .detail-wrapper {
    display: flex;
    justify-content: center;
    align-items: start;
    gap: 4rem;
    .detail-info {
      padding: 4rem 2rem;
      display: flex;
      flex-direction: column;
      gap: 2rem;
      align-items: flex-start;

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

      .add-favorite {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        height: 40px;
        background-color: var(--background-color);
        padding: 0 1rem;
        border: 1px solid rgb(70, 70, 70);
        border-radius: 0.3rem;
        color: var(--light-text-color);
        cursor: pointer;
        text-transform: uppercase;
        transition: all 0.3s ease-in-out;

        .favorite {
          display: block;
          width: 20px;
          height: 20px;
          background-image: url('../assets/images/heart.png');
          background-size: cover;
          background-position: center;
          background-repeat: no-repeat;
        }
      }
      .remove-favorite {
        background-color: var(--primary-color);
        padding: 0 1rem;
        height: 40px;
        border: none;
        border-radius: 0.3rem;
        color: var(--light-text-color);
        cursor: pointer;
        text-transform: uppercase;
        transition: all 0.3s ease-in-out;
      }
    }
  }

  .delete-palette {
    color: red;
    background-color: transparent;
    border: none;
    display: block;
    cursor: pointer;
    text-decoration: underline;
    padding: 0.5rem;
  }
}
</style>
