<template>
  <div>
    <Login v-if="!isLoggedIn" @login-success="onLoginSuccess" />
    
    <div v-else class="app-wrapper">
      <header class="app-header">
        <div class="header-content">
          <h1>CRUD de Usuários</h1>
          <button class="logout-button" @click="logout">Sair</button>
        </div>
      </header>

      <div class="container">
        <!-- Formulário para criar -->
        <div class="form-wrapper">
          <UserForm 
            v-if="!editando"
            :usuario="{nome: '', email: ''}"
            :isEditing="false"
            @submit="salvarUsuario"
          />
        </div>

        <!-- Modal para editar -->
        <Modal 
          :show="editando" 
          title="Editar Usuário"
          @close="limparFormulario"
        >
          <UserForm 
            :usuario="usuario"
            :isEditing="true"
            @submit="salvarUsuario"
          />
        </Modal>

        <div class="cards-container">
          <UserCard
            v-for="user in usuarios"
            :key="user.id"
            :user="user"
            @edit="editarUsuario"
            @delete="confirmarDeletar"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Login from './components/Login.vue'
import UserForm from './components/UserForm.vue'
import UserCard from './components/UserCard.vue'
import Modal from './components/Modal.vue'

export default {
  name: 'App',
  
  components: {
    Login,
    UserForm,
    UserCard,
    Modal
  },

  data() {
    return {
      isLoggedIn: false,
      usuarios: [],
      usuario: {
        nome: '',
        email: ''
      },
      editando: false
    }
  },

  created() {
    const token = localStorage.getItem('token')
    if (token) {
      this.isLoggedIn = true
      this.setupAxiosInterceptor()
      this.carregarUsuarios()
    }
  },

  methods: {
    async carregarUsuarios() {
      try {
        const response = await axios.get('http://localhost:8080/api/usuarios')
        this.usuarios = response.data
      } catch (error) {
        console.error('Erro ao carregar usuários:', error)
      }
    },

    setupAxiosInterceptor() {
      axios.interceptors.request.use(config => {
        const token = localStorage.getItem('token')
        if (token) {
          config.headers.Authorization = `Bearer ${token}`
        }
        return config
      })
    },

    async salvarUsuario(dados) {
      try {
        if (this.editando) {
          await axios.put(`http://localhost:8080/api/usuarios/${this.usuario.id}`, dados)
        } else {
          // Aguarde a resposta do POST e só então atualize a lista
          const response = await axios.post('http://localhost:8080/api/usuarios', dados)
          if (response.data) {
            // Ao invés de chamar carregarUsuarios, adicione o novo usuário à lista
            this.usuarios.push(response.data)
          }
        }
        this.limparFormulario()
      } catch (error) {
        console.error('Erro ao salvar usuário:', error)
        alert('Erro ao salvar usuário. Por favor, tente novamente.')
      }
    },

    editarUsuario(user) {
      this.usuario = { ...user }
      this.editando = true
    },

    async confirmarDeletar(id) {
      if (confirm('Tem certeza que deseja deletar este usuário?')) {
        try {
          await axios.delete(`http://localhost:8080/api/usuarios/${id}`)
          await this.carregarUsuarios()
        } catch (error) {
          console.error('Erro ao deletar usuário:', error)
        }
      }
    },

    onLoginSuccess() {
      this.isLoggedIn = true
      this.setupAxiosInterceptor()
      this.carregarUsuarios()
    },

    logout() {
      localStorage.removeItem('token')
      this.isLoggedIn = false
      this.usuarios = []
    },

    limparFormulario() {
      this.usuario = { nome: '', email: '' }
      this.editando = false
    }
  }
}
</script>

<style>
.app-wrapper {
  background-color: #f5f5f5;
  min-height: 100vh;
}

.app-header {
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.app-header h1 {
  margin: 0;
  font-size: 1.8rem;
  color: #333;
}

.logout-button {
  background-color: #f44336;
  color: white;
  padding: 0.3rem 0.8rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: background-color 0.2s;
}

.logout-button:hover {
  background-color: #d32f2f;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.form-wrapper {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  margin-bottom: 2rem;
}

.cards-container {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
}

@media (max-width: 768px) {
  .header-content {
    padding: 1rem;
  }

  .container {
    padding: 0 1rem;
  }

  .form-wrapper {
    padding: 1rem;
    margin: 1rem;
  }

  .cards-container {
    grid-template-columns: 1fr;
    gap: 1rem;
    margin: 0 1rem;
  }
}

@media (max-width: 480px) {
  .app-header h1 {
    font-size: 1.5rem;
  }

  .logout-button {
    padding: 0.25rem 0.6rem;
    font-size: 0.75rem;
  }
}
</style>