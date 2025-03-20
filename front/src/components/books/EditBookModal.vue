<template>
  <div v-if="isModalOpen" class="modal">
    <div class="modal-content">
      <h3>Editar Livro</h3>
      <form @submit.prevent="saveChanges">
        <div class="content">
          <label for="title">Título:</label>
          <input v-model="editedBook.title" type="text" id="title" required />
        </div>

        <div class="content">
          <label for="author">Autor:</label>
          <input v-model="editedBook.author" type="text" id="author" required />
        </div>

        <div class="content">
          <label for="description">Descrição:</label>
          <textarea v-model="editedBook.description" id="description" required></textarea>
        </div>

        <div class="button-area">
          <button type="submit">Salvar Alterações</button>
          <button type="button" @click="closeEditModal">Fechar</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    book: {
      type: Object,
      required: true
    },
    isModalOpen: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      editedBook: { ...this.book }
    };
  },
  methods: {
    closeEditModal() {
      this.$emit("close-modal");
    },
    saveChanges() {
      this.$emit("save-changes", this.editedBook);
      this.$emit("close-modal");
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
}

.modal-content form {
  display: flex;
  flex-direction: column;
}

.modal-content input,
.modal-content textarea {
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #0c66a3;
  border-radius: 5px;
}

.button-area {
  display: flex;
}

.modal-content button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 5px;
  width: 100%;
}

.modal-content button:hover {
  background-color: #45a049;
}

.modal-content button:nth-child(2) {
  background-color: #f44336;
}

.modal-content button:nth-child(2):hover {
  background-color: #d32f2f;
}

.content {
  display: flex;
  flex-direction: column;
}
</style>
