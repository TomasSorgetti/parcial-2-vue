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
footer p {
  text-align: center;
  padding: 3rem 0;
}
</style>
