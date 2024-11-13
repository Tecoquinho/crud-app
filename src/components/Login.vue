<template>
    <div class="login-container">
      <div class="login-box">
        <h2>Login</h2>
        <div v-if="error" class="error-message">
          {{ error }}
        </div>
        <form @submit.prevent="login" class="login-form">
          <div class="form-group">
            <label>Usuário</label>
            <input 
              type="text" 
              v-model="credentials.username" 
              placeholder="Digite seu usuário"
              required
            >
          </div>
          <div class="form-group">
            <label>Senha</label>
            <input 
              type="password" 
              v-model="credentials.password" 
              placeholder="Digite sua senha"
              required
            >
          </div>
          <button type="submit" :disabled="loading">
            {{ loading ? 'Carregando...' : 'Entrar' }}
          </button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'
  
  export default {
    data() {
      return {
        credentials: {
          username: '',
          password: ''
        },
        error: null,
        loading: false
      }
    },
  
    methods: {
      async login() {
        this.error = null
        this.loading = true
        
        try {
          const response = await axios.post('http://localhost:8080/api/auth/login', this.credentials)
          localStorage.setItem('token', response.data.token)
          this.$emit('login-success')
        } catch (error) {
          this.error = 'Usuário ou senha inválidos'
        } finally {
          this.loading = false
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f5f5f5;
  }
  
  .login-box {
    background: white;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 400px;
  }
  
  h2 {
    text-align: center;
    color: #333;
    margin-bottom: 1.5rem;
    font-size: 1.5rem;
  }
  
  .login-form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
  
  label {
    color: #666;
    font-size: 0.9rem;
  }
  
  input {
    padding: 0.75rem;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 1rem;
    transition: border-color 0.2s;
  }
  
  input:focus {
    outline: none;
    border-color: #4CAF50;
    box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.1);
  }
  
  button {
    background-color: #4CAF50;
    color: white;
    padding: 0.75rem;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.2s;
    margin-top: 0.5rem;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }
  
  .error-message {
    background-color: #ffebee;
    color: #c62828;
    padding: 0.75rem;
    border-radius: 4px;
    margin-bottom: 1rem;
    text-align: center;
    font-size: 0.9rem;
  }
  
  /* Responsividade */
  @media (max-width: 480px) {
    .login-box {
      margin: 1rem;
      padding: 1.5rem;
    }
  }
  </style>