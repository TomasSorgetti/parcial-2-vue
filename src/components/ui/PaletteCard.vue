<script>
import { RouterLink } from 'vue-router'

export default {
  name: 'PaletteCard',
  components: {
    RouterLink,
  },
  props: {
    palette: {
      type: Object,
      required: true,
    },
    isLoading: {
      type: Boolean,
      required: true,
    },
    isFavorite: {
      type: Boolean,
      required: false,
      default: false,
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
  <RouterLink
    :to="`/palette/${palette.id}`"
    class="card"
    :style="{ borderColor: isFavorite ? 'var(--primary-color)' : '' }"
  >
    <div v-if="isFavorite" class="favorite"></div>
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
  </RouterLink>
</template>

<style>
.card {
  position: relative;
  background-color: rgba(0, 0, 0, 0.281);
  border: 1px solid var(--light-dark-color);
  padding: 1rem 2rem;
  border-radius: 20px;
  width: 100%;
  min-width: 340px;
  max-width: 360px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 1rem;
  text-decoration: none;
  color: var(--light-text-color);
  transition: background-color 0.3s ease;

  &:hover {
    background-color: rgba(0, 0, 0, 0.7);
  }

  .favorite {
    width: 30px;
    height: 30px;
    position: absolute;
    top: 1rem;
    right: 1rem;
    background-image: url('../../assets/images/heart.png');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
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
</style>
