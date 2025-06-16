<script>
export default {
  name: 'Foo-ter',
  components: {},
}
</script>

<template>
  <footer>
    <p>
      &copy; {{ new Date().getFullYear() }} Tomás Sorgetti - El mejor gestor de paletas de la
      historia
    </p>
  </footer>
</template>

<style>
footer {
  background-color: var(--background-color);
  p {
    text-align: center;
    padding: 3rem 0;
  }
}
</style>
