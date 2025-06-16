<script>
import PaletteCreator from '@/components/sections/PaletteCreator.vue'
import PaletteHistory from '@/components/sections/PaletteHistory.vue'
import SelectedPalette from '@/components/sections/SelectedPalette.vue'

export default {
  name: 'HomeView',
  components: {
    PaletteCreator,
    SelectedPalette,
    PaletteHistory,
  },
  inject: ['isLoading', 'setLoading', 'showConfirm', 'showAlert'],
  data() {
    return {
      palettes: [],
      selectedPalette: [],
      categories: [
        { value: 'warm', label: 'Cálida' },
        { value: 'cool', label: 'Fría' },
        { value: 'neutral', label: 'Neutra' },
        { value: 'vibrant', label: 'Vibrante' },
      ],
      modes: [
        { value: 'light', label: 'Claro' },
        { value: 'dark', label: 'Oscuro' },
      ],
      favorites: [],
    }
  },

  methods: {
    createPalette(palette) {
      const paletteData = { ...palette }
      const paletteSelected = [...this.selectedPalette]
      this.selectedPalette = []
      this.setLoading(true)
      setTimeout(() => {
        const newPalette = {
          id: Date.now(),
          name: paletteData.name,
          description: paletteData.description,
          category: this.categories.find((category) => category.value === paletteData.category)
            .label,
          mode: paletteData.mode === 'dark' ? 'Oscuro' : 'Claro',
          colors: paletteSelected,
        }
        this.palettes.push(newPalette)
        localStorage.setItem('palettes', JSON.stringify(this.palettes))
        this.setLoading(false)
        this.showAlert('¡Paleta creada exitosamente!')
      }, 1500)
    },
    deletePalette(id) {
      this.showConfirm('¿Estás seguro de eliminar esta paleta?', () => {
        this.setLoading(true)
        setTimeout(() => {
          this.palettes = this.palettes.filter((palette) => palette.id !== id)
          localStorage.setItem('palettes', JSON.stringify(this.palettes))
          this.setLoading(false)
          this.showAlert('Paleta eliminada exitosamente')
        }, 500)
      })
    },
    setSelectedPalette(palette) {
      this.selectedPalette = palette
    },
  },

  mounted() {
    // obtener paletas del local storage
    const storedPalettes = localStorage.getItem('palettes')
    if (storedPalettes) {
      this.palettes = JSON.parse(storedPalettes)
    }

    // obtener favoritos del local storage
    const storedFavorites = localStorage.getItem('favorites')
    if (storedFavorites) {
      this.favorites = JSON.parse(storedFavorites)
    }
  },
}
</script>

<template>
  <main>
    <section class="banner">
      <div class="banner-wrapper">
        <div class="banner-content">
          <h1>Gestor de paletas de colores</h1>
          <!-- componente de creacion de paletas -->
          <PaletteCreator
            :isLoading="isLoading"
            :categories="categories"
            :modes="modes"
            :set-selected-palette="setSelectedPalette"
            :create-palette="createPalette"
          />
        </div>
        <!-- componente de paletas -->
        <SelectedPalette :selectedPalette="selectedPalette" />
      </div>
    </section>
    <!-- componente de historial depaletas -->
    <PaletteHistory
      :isLoading="isLoading"
      :palettes="palettes"
    />
  </main>
</template>

<style>
main {
  .banner {
    background-image: url('../assets/images/banner_bg.png');
    background-position: center top;
    background-repeat: no-repeat;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .banner-wrapper {
      height: 100%;
      width: 100%;
      max-width: 1280px;
      padding-top: 10rem;
      display: flex;
      justify-content: space-between;

      .banner-content {
        display: flex;
        flex-direction: column;
        gap: 3rem;

        h1 {
          font-size: var(--font-size-5xl);
          font-weight: bold;
          text-transform: uppercase;
          max-width: 700px;
        }
      }
    }
  }
}
</style>
