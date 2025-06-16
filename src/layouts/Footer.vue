<script>
export default {
  name: 'Foo-ter',
  components: {},
}
</script>

<template>
  <footer>
    <p>&copy; {{ new Date().getFullYear() }} Tomás Sorgetti</p>
  </footer>
</template>

<style>
footer p {
  text-align: center;
  padding: 3rem 0;
}
</style>
