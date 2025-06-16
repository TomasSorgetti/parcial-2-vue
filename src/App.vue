<script>
import { RouterView } from 'vue-router'
import Navbar from './layouts/Navbar.vue'
import AppFooter from './layouts/Footer.vue'

export default {
  name: 'App',
  components: {
    RouterView,
    Navbar,
    AppFooter,
  },
}
</script>

<template>
  <Navbar />
  <RouterView />
  <AppFooter />
</template>

<style scoped></style>
