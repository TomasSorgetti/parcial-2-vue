<script>
import { getRandomPalette } from '@/lib/utils/palette'

export default {
  name: 'PaletteCreator',
  props: {
    isLoading: {
      type: Boolean,
      required: true,
    },
    categories: {
      type: Array,
      required: true,
    },
    modes: {
      type: Array,
      required: true,
    },
    createPalette: {
      type: Function,
      required: true,
    },
    setSelectedPalette: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      formData: {
        name: '',
        description: '',
        category: '',
        mode: '',
      },
      errorForm: {
        name: '',
        description: '',
        category: '',
        mode: '',
      },
    }
  },
  watch: {
    'formData.category': function () {
      if (this.formData.category && this.formData.mode) {
        const randomPalette = getRandomPalette(this.formData.category, this.formData.mode)
        this.setSelectedPalette(randomPalette.colors)
      }
    },
    'formData.mode': function () {
      if (this.formData.category && this.formData.mode) {
        const randomPalette = getRandomPalette(this.formData.category, this.formData.mode)
        this.setSelectedPalette(randomPalette.colors)
      }
    },
  },
  methods: {
    submitForm() {
      this.validateForm()

      if (
        this.errorForm.name ||
        this.errorForm.description ||
        this.errorForm.category ||
        this.errorForm.mode
      ) {
        return
      }

      if (
        this.formData.mode &&
        this.formData.category &&
        this.formData.name &&
        this.formData.description &&
        !this.isLoading
      ) {
        this.createPalette(this.formData)
        this.resetForm()
      }
    },
    resetForm() {
      this.formData.name = ''
      this.formData.description = ''
      this.formData.category = ''
      this.formData.mode = ''
    },
    validateInput(field, value) {
      switch (field) {
        case 'name':
          if (!value) {
            this.errorForm.name = 'El nombre es obligatorio'
            return this.errorForm.name
          }
          if (value.length < 3) {
            this.errorForm.name = 'El nombre debe tener al menos 3 caracteres'
            return this.errorForm.name
          }
          if (value.length > 20) {
            this.errorForm.name = 'El nombre debe tener menos de 20 caracteres'
            return this.errorForm.name
          }
          this.errorForm.name = ''
          break

        case 'description':
          if (!value) {
            this.errorForm.description = 'La descripción es obligatoria'
            return this.errorForm.description
          }
          if (value.length < 10) {
            this.errorForm.description = 'La descripción debe tener al menos 10 caracteres'
            return this.errorForm.description
          }
          if (value.length > 40) {
            this.errorForm.description = 'La descripción debe tener menos de 40 caracteres'
            return this.errorForm.description
          }
          this.errorForm.description = ''
          break

        case 'category':
          if (!value) {
            this.errorForm.category = 'La categoría es obligatoria'
            return this.errorForm.category
          }
          this.errorForm.category = ''
          break

        case 'mode':
          if (!value) {
            this.errorForm.mode = 'El modo es obligatorio'
            return this.errorForm.mode
          }
          this.errorForm.mode = ''
          break

        default:
          this.errorForm[field] = ''
          break
      }
    },
    validateForm() {
      this.validateInput('name', this.formData.name)
      this.validateInput('description', this.formData.description)
      this.validateInput('category', this.formData.category)
      this.validateInput('mode', this.formData.mode)
    },
    updateMode(mode) {
      this.formData.mode = mode
      this.validateInput('mode', mode)
    },
  },
}
</script>

<template>
  <form @submit.prevent="submitForm" class="palette-creator">
    <div class="form-field">
      <label for="name">Nombre de paleta:</label>
      <input
        id="name"
        v-model.trim="formData.name"
        type="text"
        placeholder="La mejor paleta de todas"
        @blur="validateInput('name', formData.name)"
      />
      <small>{{ errorForm.name }}</small>
    </div>

    <div class="form-field">
      <label for="description">Descripción:</label>
      <input
        id="description"
        v-model.trim="formData.description"
        type="text"
        placeholder="Descripción de la mejor paleta"
        @blur="validateInput('description', formData.description)"
      />
      <small>{{ errorForm.description }}</small>
    </div>

    <div class="form-field">
      <label for="category">Categoría:</label>
      <select
        id="category"
        v-model.trim="formData.category"
        @change="validateInput('category', formData.category)"
      >
        <option value="" disabled>Selecciona una categoría</option>
        <option
          v-bind:key="category.value"
          v-for="category in categories"
          v-bind:value="category.value"
        >
          {{ category.label }}
        </option>
      </select>
      <small>{{ errorForm.category }}</small>
    </div>
    <div class="form-field">
      <label>Modo:</label>
      <div class="checkbox-group">
        <label v-bind:key="mode.value" v-for="mode in modes">
          <input
            type="checkbox"
            v-bind:checked="formData.mode === mode.value ? true : false"
            class="checkbox"
            @change="updateMode(mode.value)"
          />
          {{ mode.label }}
        </label>
      </div>
      <small>{{ errorForm.mode }}</small>
    </div>

    <button type="submit" class="create-palette" v-bind:disabled="isLoading">Crear paleta</button>
  </form>
</template>

<style>
.palette-creator {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  max-width: 400px;

  .form-field {
    position: relative;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;

    label {
      font-size: var(--font-size-lg);
      font-weight: light;
    }

    input,
    select {
      height: 42px;
      padding: 0.5rem;
      background-color: var(--light-dark-color);
      border: none;
      color: var(--light-text-color);
      border-radius: 0.5rem;
      font-size: var(--font-size-lg);
      font-weight: light;
    }

    .checkbox-group {
      display: flex;
      gap: 1rem;

      label {
        display: flex;
        align-items: center;
        gap: 0.5rem;
      }
      .checkbox {
        width: 20px;
        height: 20px;
        cursor: pointer;
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        border-radius: 4px;
        background-color: var(--light-dark-color);
        position: relative;

        &:checked {
          background-color: var(--primary-color);
          border-color: var(--primary-color);

          &::after {
            content: '';
            position: absolute;
            top: 2px;
            left: 6px;
            width: 5px;
            height: 10px;
            border: solid white;
            border-width: 0 2px 2px 0;
            transform: rotate(45deg);
          }
        }
      }
    }

    small {
      color: var(--error-color);
      position: absolute;
      left: 0;
      bottom: -1.2rem;
    }
  }

  .create-palette {
    height: 42px;
    padding: 0.5rem;
    background-color: var(--primary-color);
    border: none;
    color: var(--light-text-color);
    border-radius: 0.5rem;
    font-size: var(--font-size-lg);
    font-weight: light;
    cursor: pointer;
    transition: all 0.3s ease-in-out;

    &:hover {
      opacity: 0.7;
    }
  }
}
</style>
