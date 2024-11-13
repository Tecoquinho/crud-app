<template>
    <div class="user-card">
      <div class="user-info">
        <div class="info-group">
          <label>Nome:</label>
          <span>{{ user.nome }}</span>
        </div>
        <div class="info-group">
          <label>Email:</label>
          <span class="email-text">{{ user.email }}</span>
        </div>
      </div>
      <div class="card-actions">
        <button class="edit-btn" @click="$emit('edit', user)">
          <i class="fas fa-edit"></i>
          Editar
        </button>
        <button class="delete-btn" @click="$emit('delete', user.id)">
          <i class="fas fa-trash"></i>
          Deletar
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'UserCard',
    props: {
      user: {
        type: Object,
        required: true,
        validator: function(obj) {
          return obj.nome !== undefined && obj.email !== undefined
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .user-card {
    background: white;
    border-radius: 8px;
    padding: 1.5rem;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    transition: transform 0.2s, box-shadow 0.2s;
  }
  
  .user-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
  
  .user-info {
    margin-bottom: 1.5rem;
  }
  
  .info-group {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.75rem 0;
    border-bottom: 1px solid #eee;
  }
  
  .info-group:last-child {
    border-bottom: none;
  }
  
  .info-group label {
    color: #666;
    font-weight: 500;
    font-size: 0.9rem;
  }
  
  .info-group span {
    color: #333;
    font-weight: 400;
    word-break: break-word;
    text-align: right;
    max-width: 60%;
  }
  
  .email-text {
    font-size: 0.9rem;
  }
  
  .card-actions {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-top: 1rem;
  }
  
  .edit-btn, .delete-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    padding: 0.75rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
    font-size: 0.9rem;
  }
  
  .edit-btn {
    background-color: #2196F3;
    color: white;
  }
  
  .edit-btn:hover {
    background-color: #1976D2;
  }
  
  .delete-btn {
    background-color: #f44336;
    color: white;
  }
  
  .delete-btn:hover {
    background-color: #d32f2f;
  }
  
  .edit-btn:active, .delete-btn:active {
    transform: scale(0.98);
  }
  
  /* Responsivo */
  @media (max-width: 768px) {
    .user-card {
      padding: 1.25rem;
    }
  
    .info-group {
      padding: 0.5rem 0;
    }
  
    .card-actions {
      gap: 0.75rem;
    }
  
    .edit-btn, .delete-btn {
      padding: 0.6rem;
      font-size: 0.85rem;
    }
  }
  
  /* Mobile específico */
  @media (max-width: 480px) {
    .user-card {
      padding: 1rem;
    }
  
    .info-group label {
      font-size: 0.85rem;
    }
  
    .info-group span {
      font-size: 0.85rem;
    }
  
    .email-text {
      font-size: 0.8rem;
    }
  
    .card-actions {
      gap: 0.5rem;
    }
  
    .edit-btn, .delete-btn {
      padding: 0.5rem;
      font-size: 0.8rem;
    }
  }
  
  /* Tratamento para emails longos */
  @media (max-width: 400px) {
    .info-group {
      flex-direction: column;
      align-items: flex-start;
      gap: 0.25rem;
    }
  
    .info-group span {
      max-width: 100%;
      text-align: left;
    }
  }
  </style>