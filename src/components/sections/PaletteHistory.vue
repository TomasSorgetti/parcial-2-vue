<script>
import PaletteCard from '../ui/PaletteCard.vue'
export default {
  name: 'PaletteHistory',
  components: {
    PaletteCard,
  },
  props: {
    isLoading: {
      type: Boolean,
      required: true,
    },
    palettes: {
      type: Array,
      required: true,
    },
    deletePalette: {
      type: Function,
      required: true,
    },
    favorites: {
      type: Array,
      required: true,
    },
  },
}
</script>

<template>
  <section class="palette-history">
    <div class="history-wrapper">
      <h2>Historial de paletas</h2>
      <div class="palette-list">
        <p v-if="palettes.length === 0">No hay paletas en el historial. Creá una!</p>
        <PaletteCard
          v-bind:key="palette.id"
          v-else
          v-for="palette in palettes"
          :palette="palette"
          :deletePalette="deletePalette"
          :isLoading="isLoading"
        ></PaletteCard>
      </div>
    </div>
  </section>
</template>

<style>
.palette-history {
  margin-top: 16rem;
  min-height: 80vh;
  background-image: url('../../assets/images/history_bg.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  padding: 6rem 0;

  .history-wrapper {
    width: 100%;
    max-width: 1280px;
    margin: 0 auto;

    h2 {
      font-size: var(--font-size-3xl);
      text-transform: uppercase;
    }
    .palette-list {
      margin-top: 4rem;
      display: flex;
      flex-wrap: wrap;
      gap: 2rem;
    }
  }
}
</style>
