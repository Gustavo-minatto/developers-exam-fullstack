<template>
  <div class="book-form">
    <h2>Cadastrar Livro</h2>
    <form @submit.prevent="submitBook">
      <div class="content">
        <label for="title">Título:</label>
        <input v-model="book.title" type="text" id="title" required />
        <div v-if="titleError" class="error-message">{{ titleError }}</div>
      </div>

      <div class="content">
        <label for="author">Autor:</label>
        <input v-model="book.author" type="text" id="author" required />
        <div v-if="authorError" class="error-message">{{ authorError }}</div>
      </div>

      <div class="content">
        <label for="description">Descrição:</label>
        <textarea v-model="book.description" id="description" required></textarea>
        <div v-if="descriptionError" class="error-message">{{ descriptionError }}</div>
      </div>

      <button type="submit" :disabled="isSendingEmail">Cadastrar</button>

      <div v-if="isSendingEmail" class="loading-message">Enviando e-mail...</div>
      <div v-if="emailSent" class="success-message">E-mail enviado com sucesso!</div>
      <div v-if="emailError" class="error-message">{{ emailError }}</div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      book: {
        id: '',
        title: '',
        author: '',
        description: ''
      },
      titleError: null,
      authorError: null,
      descriptionError: null,
      emailSent: false,
      emailError: null,
      isSendingEmail: false
    };
  },
  methods: {
    submitBook() {
      if (this.isTitleDuplicate(this.book.title)) {
        this.titleError = 'O título do livro já existe.';
        return;
      } else {
        this.titleError = null;
      }

      if (this.book.title.length < 10 || this.book.title.length > 100) {
        this.titleError = 'O título deve ter entre 10 e 100 caracteres.';
        return;
      } else {
        this.titleError = null;
      }

      if (this.book.author.length < 10 || this.book.author.length > 100) {
        this.authorError = 'O autor deve ter entre 10 e 100 caracteres.';
        return;
      } else {
        this.authorError = null;
      }

      if (this.book.description.length > 1024) {
        this.descriptionError = 'A descrição não pode ter mais de 1024 caracteres.';
        return;
      } else {
        this.descriptionError = null;
      }

      this.book.id = this.generateUniqueId();

      this.$emit('add-book', this.book);

      this.sendEmailNotification(this.book);

      console.log('Livro adicionado:', JSON.stringify(this.book, null, 2));

      this.resetForm();
    },
    resetForm() {
      this.book = { title: '', author: '', description: '' };
    },
    isTitleDuplicate(title) {
      const existingBooks = [
        { title: 'O Senhor dos Anéis' },
        { title: '1984' }
      ];
      return existingBooks.some(book => book.title === title);
    },
    generateUniqueId() {
      return `book-${Date.now()}`;
    },
    async sendEmailNotification(book) {
      this.isSendingEmail = true;
      this.emailSent = false;
      this.emailError = null;

      try {
        const response = await fetch('https://api.example.com/send-email', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            to: 'developers@inspand.com.br',
            subject: `Novo Livro Cadastrado: ${book.title}`,
            body: `Novo livro cadastrado:\n\nTítulo: ${book.title}\nAutor: ${book.author}\nDescrição: ${book.description}`
          })
        });

        if (response.ok) {
          console.log('E-mail enviado com sucesso');
          this.emailSent = true;
        } else {
          throw new Error('Erro ao enviar o e-mail');
        }
      } catch (error) {
        console.error('Falha no envio do e-mail:', error.message);
      } finally {
        this.isSendingEmail = false;
      }
    }
  }
};
</script>

<style scoped>
.book-form {
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #0c66a3;
  border-radius: 8px;
  background: #fff;
}

h2 {
  margin: 10px 0;
  text-align: center;
}

.content {
  display: flex;
  flex-direction: column;
}

.cadastro {
  display: flex;
  flex-direction: column;
}

input,
textarea {
  border: 1px solid #0c66a3;
  border-radius: 10px;
  margin-bottom: 10px;
  height: 40px;
  padding: 10px;
}

textarea {
  width: 400px;
  max-width: 100%;
}

button {
  background-color: #4CAF50;
  height: 50px;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #45a049;
  transition: 0.5s;
}

form {
  display: flex;
  align-items: center;
  gap: 10px;
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}

.content {
  display: flex;
  flex-direction: column;
}
</style>
