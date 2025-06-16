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
      <RouterLink to="/">Logo</RouterLink>

      <!-- Hamburger Menu -->
      <button class="hamburger">
        <div class="bar"></div>
        <div class="bar"></div>
        <div class="bar"></div>
      </button>

      <!-- Menu -->
      <ul>
        <li>
          <RouterLink to="/">Home</RouterLink>
        </li>
        <li>
          <RouterLink to="/">Products</RouterLink>
        </li>
        <li>
          <RouterLink to="/">Contact</RouterLink>
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
  background-color: gray;

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 20px;
    max-width: 1100px;
    margin: 0 auto;

    .hamburger {
      display: none;
    }
  }
}
</style>
