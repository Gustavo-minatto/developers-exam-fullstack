<template>
  <div class="book-card">
    <h2>{{ book.title }}</h2>
    <p><strong>Autor:</strong> {{ book.author }}</p>
    <p> <strong>Descrição:</strong> {{ book.description }}</p>

    <div class="actions">
      <button @click="openEditModal">Alterar</button>
      <button @click="deleteBook">Excluir</button>
    </div>

    <EditBookModal v-if="isModalOpen" :book="book" :isModalOpen="isModalOpen" @close-modal="closeEditModal"
      @save-changes="saveChanges" />
  </div>
</template>

<script>
import EditBookModal from './EditBookModal.vue';

export default {
  components: {
    EditBookModal
  },
  props: {
    book: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isModalOpen: false
    };
  },
  methods: {
    openEditModal() {
      this.isModalOpen = true;
    },
    closeEditModal() {
      this.isModalOpen = false; // Fechar o modal
    },
    saveChanges(updatedBook) {
      this.$emit('edit-book', updatedBook);
      this.closeEditModal(); // Fechar o modal imediatamente após salvar
    },
    deleteBook() {
      this.$emit('delete-book', this.book.id);
    }
  }
}
</script>


<style scoped>
.book-card {
  border: 1px solid #0c66a3;
  border-radius: 8px;
  padding: 1rem;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.book-card h2 {
  color: #0c66a3;
  margin-top: 0;
}

.actions {
  margin-top: 10px;
  display: flex;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 5px;
  width: 100%;
}

button:hover {
  background-color: #45a049;
}

button:nth-child(2) {
  background-color: #f44336;
}

button:nth-child(2):hover {
  background-color: #d32f2f;
}
</style>
