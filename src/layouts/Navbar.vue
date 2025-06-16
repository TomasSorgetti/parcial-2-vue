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
      <RouterLink to="/" class="logo">Palette Creator</RouterLink>

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
  background-color: rgba(0, 0, 0, 0.651);
  z-index: 999;
  border-bottom: 1px solid var(--light-dark-color);

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem 0;
    max-width: 1280px;
    margin: 0 auto;

    .logo {
      color: var(--light-text-color);
      text-decoration: none;
      font-size: var(--font-size-2xl);
      font-weight: bold;
      text-transform: uppercase;
    }
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
          text-transform: uppercase;
          font-weight: 300;
          transition: all 0.3s ease-in-out;

          &:hover {
            color: var(--primary-color);
          }
        }
      }
    }
  }
}
</style>
