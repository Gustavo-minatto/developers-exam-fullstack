<template>
  <div class="book-list">
    <h1>Lista de Livros</h1>

    <book-form @add-book="addBook" />

    <div v-if="error" class="error-message">
      {{ error }}
    </div>

    <div v-if="loading" class="loading">
      Carregando...
    </div>

    <div v-else>
      <div class="books-container">
        <book-card v-for="book in books" :key="book.id" :book="book" @edit-book="editBook" @delete-book="deleteBook" />
      </div>

      <div v-if="books.length === 0" class="no-books">
        Nenhum livro encontrado.
      </div>
    </div>

    <edit-book-modal v-if="isModalOpen" :book="bookToEdit" :isModalOpen="isModalOpen" @close-modal="closeModal"
      @save-changes="saveBookChanges" />
  </div>
</template>

<script>
import BookCard from '@/components/books/BookCard.vue';
import EditBookModal from '@/components/books/EditBookModal.vue';
import BookForm from '@/components/books/BookForm.vue';

export default {
  components: {
    BookCard,
    EditBookModal,
    BookForm
  },
  data() {
    return {
      books: JSON.parse(localStorage.getItem('books')) || [],
      error: null,
      loading: false,
      isModalOpen: false,
      bookToEdit: null
    };
  },
  methods: {
    editBook(book) {
      this.bookToEdit = { ...book };
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },
    saveBookChanges(updatedBook) {
      const index = this.books.findIndex(book => book.id === updatedBook.id);
      if (index !== -1) {
        this.books.splice(index, 1, updatedBook);
        this.saveBooksToLocalStorage();
      }
      this.closeModal();
    },
    deleteBook(bookId) {
      this.books = this.books.filter(book => book.id !== bookId);
      this.saveBooksToLocalStorage();
    },
    addBook(book) {
      this.books.push(book);
      this.saveBooksToLocalStorage();
    },
    saveBooksToLocalStorage() {
      localStorage.setItem('books', JSON.stringify(this.books));
    }
  }
};
</script>

<style scoped>
.book-list {
  max-width: 1200px;
  margin: 0 auto;
}

h1 {
  text-align: center;
}

.books-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.no-books {
  text-align: center;
  padding: 2rem;
  color: #666;
}

.error-message {
  color: red;
  text-align: center;
  padding: 1rem;
}
</style>
