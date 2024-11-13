<template>
    <form @submit.prevent="submitForm" class="form-container">
      <div class="input-row">
        <input 
          type="text" 
          v-model="formData.nome" 
          placeholder="Nome"
          class="input-field"
          required
        >
        <input 
          type="email" 
          v-model="formData.email" 
          placeholder="Email"
          class="input-field"
          required
        >
        <button type="submit" class="create-button">
          {{ isEditing ? 'Atualizar' : 'Criar' }}
        </button>
      </div>
    </form>
  </template>
  
  <script>
  export default {
    name: 'UserForm',
    
    props: {
      usuario: {
        type: Object,
        default: () => ({ 
          nome: '', 
          email: '' 
        })
      },
      isEditing: {
        type: Boolean,
        default: false
      }
    },
    
    data() {
      return {
        formData: {
          nome: '',
          email: ''
        }
      }
    },
    
    watch: {
      usuario: {
        handler(newValue) {
          this.formData = { ...newValue }
        },
        immediate: true
      }
    },
    
    methods: {
      submitForm() {
        if (this.formData.nome.trim() && this.formData.email.trim()) {
          this.$emit('submit', { ...this.formData })
          if (!this.isEditing) {
            this.formData = { nome: '', email: '' }
          }
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .form-container {
    padding: 1.5rem;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  }
  
  .input-row {
    display: flex;
    gap: 1rem;
    align-items: center;
  }
  
  .input-field {
    flex: 1;
    height: 42px;
    padding: 0 1rem;
    border: 1px solid #e0e0e0;
    border-radius: 6px;
    font-size: 0.95rem;
    color: #333;
    background-color: #fff;
    transition: all 0.2s ease;
  }
  
  .input-field:hover {
    border-color: #bbbbbb;
  }
  
  .input-field:focus {
    border-color: #2196F3;
    box-shadow: 0 0 0 2px rgba(33, 150, 243, 0.1);
    outline: none;
  }
  
  .input-field::placeholder {
    color: #999;
    font-size: 0.9rem;
  }
  
  .create-button {
    height: 42px;
    padding: 0 2rem;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
    white-space: nowrap;
    min-width: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .create-button:hover {
    background-color: #45a049;
    transform: translateY(-1px);
  }
  
  .create-button:active {
    transform: translateY(0);
  }
  
  /* Estilo para o modo de edição */
  .form-container.editing .create-button {
    background-color: #2196F3;
  }
  
  .form-container.editing .create-button:hover {
    background-color: #1976D2;
  }
  
  /* Responsividade */
  @media (max-width: 768px) {
    .form-container {
      padding: 1rem;
    }
  
    .input-row {
      flex-direction: column;
      gap: 0.75rem;
    }
  
    .input-field {
      width: 100%;
      font-size: 0.9rem;
    }
  
    .create-button {
      width: 100%;
      padding: 0 1rem;
    }
  }
  
  /* Mobile específico */
  @media (max-width: 480px) {
    .form-container {
      margin: 0;
      border-radius: 0;
    }
  
    .input-field {
      height: 40px;
      font-size: 0.85rem;
    }
  
    .input-field::placeholder {
      font-size: 0.85rem;
    }
  
    .create-button {
      height: 40px;
      font-size: 0.9rem;
    }
  }
  
  /* Animações */
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .form-container {
    animation: fadeIn 0.3s ease;
  }
  </style>