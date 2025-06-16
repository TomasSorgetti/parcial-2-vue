<script>
export default {
  name: 'GlobalModal',
  props: {
    show: {
      type: Boolean,
      required: true,
    },
    message: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: 'alert',
    },
  },
  methods: {
    confirm() {
      this.$emit('confirm')
      this.close()
    },
    close() {
      this.$emit('close')
    },
    handleOutsideClick(event) {
      if (event.target.classList.contains('modal')) {
        this.close()
      }
    },
  },
}
</script>

<template>
  <div v-if="show" class="modal" @click="handleOutsideClick">
    <div class="modal-content">
      <p>{{ message }}</p>
      <div class="modal-buttons">
        <button v-if="type === 'alert'" @click="close" class="modal-button-confirm modal-button">
          Aceptar
        </button>
        <template v-else>
          <button @click="close" class="modal-button-cancel modal-button">Cancelar</button>
          <button @click="confirm" class="modal-button-confirm modal-button">Aceptar</button>
        </template>
      </div>
    </div>
  </div>
</template>

<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.596);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;

  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.63);
    max-width: 400px;
    width: 100%;
    min-height: 180px;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1.5rem;
    color: var(--light-dark-color);

    p {
      margin: 0 0 20px;
      font-size: 16px;
    }

    .modal-buttons {
      display: flex;
      justify-content: center;
      gap: 1rem;

      .modal-button {
        padding: 0.5rem 1rem;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;

        &:hover {
          opacity: 0.6;
        }
      }

      .modal-button-confirm {
        background-color: var(--primary-color);
        color: var(--light-text-color);
      }

      .modal-button-cancel {
        background-color: gray;
        color: var(--light-text-color);
      }
    }
  }
}
</style>
