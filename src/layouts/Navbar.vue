<script>
import { RouterLink } from 'vue-router'

export default {
  name: 'Nav-bar',
  components: {
    RouterLink,
  },
}
</script>

<template>
  <header>
    <nav>
      <!-- Logo -->
      <RouterLink to="/">Palette Creator</RouterLink>

      <!-- Hamburger Menu -->
      <button class="hamburger">
        <div class="bar"></div>
        <div class="bar"></div>
        <div class="bar"></div>
      </button>

      <!-- Menu -->
      <ul>
        <li>
          <RouterLink to="/">Inicio</RouterLink>
        </li>
        <li>
          <RouterLink to="/favorites">Favoritos</RouterLink>
        </li>
      </ul>
    </nav>
  </header>
</template>

<style>
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: var(--background-color);

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 0;
    max-width: 1280px;
    margin: 0 auto;

    .hamburger {
      display: none;
    }
    ul {
      display: flex;
      gap: 2rem;
      li {
        list-style: none;
        a {
          text-decoration: none;
          color: var(--light-text-color);

          &:hover {
            color: var(--primary-color);
          }
        }
      }
    }
  }
}
</style>
