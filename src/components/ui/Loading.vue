<script>
export default {
  name: 'LoadingSpinner',
  props: {
    isLoading: {
      type: Boolean,
      required: true,
    },
  },
}
</script>

<template>
  <div v-if="isLoading" class="loading">
    <div class="spinner"></div>
  </div>
</template>

<style>
.loading {
  position: fixed;
  top: 2rem;
  left: 50%;
  transform: translateX(-50%);
  display: block;

  .spinner {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 4px solid rgba(0, 0, 0, 0.1);
    border-top-color: var(--primary-color);
    border-left-color: var(--primary-color);
    animation: spin 1s ease-in-out infinite;
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
