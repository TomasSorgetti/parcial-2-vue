<script>
export default {
  name: 'PaletteHistory',
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
  methods: {
    copyColor(color) {
      navigator.clipboard.writeText(color)
      // this.$root.showAlert('Color copiado al portapapeles')
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
        <div v-bind:key="palette.id" v-else v-for="palette in palettes" class="palette-item">
          <button
            @click="deletePalette(palette.id)"
            v-bind:disabled="isLoading"
            class="delete-palette"
          >
            X
          </button>
          <h3>{{ palette.name }}</h3>
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
          <ul class="color-list">
            <li v-bind:key="color" v-for="color in palette.colors">
              <button
                @click="copyColor(color)"
                v-bind:style="{ backgroundColor: color }"
                class="color-circle"
              >
                <span>{{ color }}</span>
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<style>
.palette-history {
  margin-top: 16rem;
  min-height: 80vh;
  background-image: url('../assets/images/history_bg.png');
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

      .palette-item {
        position: relative;
        background-color: rgba(0, 0, 0, 0.281);
        border: 1px solid var(--light-dark-color);
        padding: 1rem 2rem;
        border-radius: 20px;
        width: 100%;
        max-width: 360px;
        min-height: 200px;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        gap: 1rem;

        .delete-palette {
          position: absolute;
          top: 0.5rem;
          right: 1rem;
          background-color: transparent;
          border: none;
          color: var(--light-text-color);
          font-size: var(--font-size-2xl);
          cursor: pointer;
        }

        h3 {
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

        .color-list {
          display: flex;
          align-items: center;
          gap: 0.5rem;

          .color-circle {
            position: relative;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: 1px solid var(--light-dark-color);
            cursor: pointer;

            span {
              position: absolute;
              top: -1.5rem;
              color: var(--light-text-color);
              background-color: #171717;
              display: none;
            }

            &:hover span {
              display: block;
            }
          }
        }
      }
    }
  }
}
</style>
