<template>
  <h1>Lista de Livros</h1>
  <div class="book-form">
    <div class="content">
      <h2>Cadastrar Livro</h2>

      <form @submit.prevent="submitBook">
        <div class="cadastro">
          <label for="title">Título:</label>
          <input v-model="book.title" type="text" id="title" required placeholder="Ex: O Senhor dos Anéis" />
          <div v-if="titleError" class="error-message">{{ titleError }}</div>
        </div>

        <div class="cadastro">
          <label for="author">Autor:</label>
          <input v-model="book.author" type="text" id="author" required placeholder="Ex: J.R.R. Tolkien" />
          <div v-if="authorError" class="error-message">{{ authorError }}</div>
        </div>

        <div class="cadastro">
          <label for="description">Descrição:</label>
          <textarea v-model="book.description" id="description" required
            placeholder="Ex: Uma história épica de fantasia."></textarea>
          <div v-if="descriptionError" class="error-message">{{ descriptionError }}</div>
        </div>

        <button type="submit" :disabled="isSendingEmail">Cadastrar</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      book: {
        title: '',
        author: '',
        description: ''
      },
      books: JSON.parse(localStorage.getItem('books')) || [],
      titleError: null,
      authorError: null,
      descriptionError: null,
      isSendingEmail: false,
      emailSent: false
    };
  },
  methods: {
    submitBook() {
      if (this.isTitleDuplicated()) {
        this.titleError = "Este título já foi cadastrado!";
        return;
      }

      if (!this.isTitleValid()) {
        this.titleError = "O título deve ter entre 10 e 100 caracteres!";
        return;
      }

      if (!this.isAuthorValid()) {
        this.authorError = "O autor deve ter entre 10 e 100 caracteres!";
        return;
      }

      if (!this.isDescriptionValid()) {
        this.descriptionError = "A descrição deve ter no máximo 1024 caracteres!";
        return;
      }

      this.books.push(this.book);

      localStorage.setItem('books', JSON.stringify(this.books));

      this.sendEmailNotification(this.book);

      this.resetForm();
    },
    isTitleDuplicated() {
      return this.books.some(book => book.title === this.book.title);
    },
    isTitleValid() {
      return this.book.title.length >= 10 && this.book.title.length <= 100;
    },
    isAuthorValid() {
      return this.book.author.length >= 10 && this.book.author.length <= 100;
    },
    isDescriptionValid() {
      return this.book.description.length <= 1024;
    },
    resetForm() {
      this.book = { title: '', author: '', description: '' };
      this.titleError = null;
      this.authorError = null;
      this.descriptionError = null;
    },
    async sendEmailNotification(book) {
      this.isSendingEmail = true;
      this.emailSent = false;

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
  @media (max-width: 800px) {
    width: auto;
  }
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

  @media (max-width: 800px) {
    flex-direction: column;
  }
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}

@media (max-width: 800px) {}
</style>
