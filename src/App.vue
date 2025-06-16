<script>
import { RouterView } from 'vue-router'
import Navbar from './layouts/Navbar.vue'
import AppFooter from './layouts/Footer.vue'
import Modal from './components/ui/Modal.vue'
import Loading from './components/ui/Loading.vue'

export default {
  name: 'App',
  components: {
    RouterView,
    Navbar,
    AppFooter,
    Modal,
    Loading,
  },
  data() {
    return {
      isLoading: false,
      modal: {
        show: false,
        message: '',
        type: 'alert',
        onConfirm: null,
      },
    }
  },
  methods: {
    // Global
    setLoading(value) {
      this.isLoading = value
    },

    // Modal
    showAlert(message) {
      this.modal = {
        show: true,
        message,
        type: 'alert',
        onConfirm: null,
      }
    },

    showConfirm(message, onConfirm) {
      this.modal = {
        show: true,
        message,
        type: 'confirm',
        onConfirm,
      }
    },

    closeModal() {
      this.modal.show = false
    },

    handleConfirm() {
      if (this.modal.onConfirm) {
        this.modal.onConfirm()
      }
      this.closeModal()
    },
  },

  provide() {
    return {
      isLoading: this.isLoading,
      setLoading: this.setLoading,
      showConfirm: this.showConfirm,
      showAlert: this.showAlert,
    }
  },
}
</script>

<template>
  <Navbar />
  <RouterView />
  <AppFooter />

  <Loading :isLoading="isLoading" />

  <Modal
    v-if="modal.show"
    :show="modal.show"
    :message="modal.message"
    :type="modal.type"
    @close="closeModal"
    @confirm="handleConfirm"
  />
</template>

<style scoped></style>
