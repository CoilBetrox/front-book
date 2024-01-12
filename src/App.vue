<template>
  <div id="app" class="container">
      <div class="row">
          <div class="col-md-12 mt-2">
              <h1>Books</h1>
          </div>
      </div>
      <div class="row">
          <div class="col-md-12">
              <formulario-book @crear-usuario="postUsuario" />
              <tabla-books :books="books" @eliminar-usuario="deleteUsuario" @actualizar-usuario="putUsuario" />
          </div>
      </div>
  </div>
</template>

<script>

import TablaBooks from '@/components/TablaBooks.vue';
import FormularioBook from '@/components/FormularioBook.vue';


export default {
  name: "app",
  data() {
      return {
          books: [],
      }
  },
  components: {
    TablaBooks,
    FormularioBook
  },
  methods: {
      async getUsuarios() {
          try {
              const response = await fetch('http://localhost:8081/books');
              console.log(response);
              this.books = await response.json();
          } catch (error) {
              console.error(error);
          }
      },
      async postUsuario(book) {
          try {
              const response = await fetch('http://localhost:8081/books', {
                  method: 'POST',
                  body: JSON.stringify(book),
                  headers: { 'Content-type': 'application/json; charset=UTF-8' },
              });
              
              const usuarioCreado = await response.json();
              this.books = [...this.books, usuarioCreado];
          } catch (error) {
              console.error(error);
          }
      },
      async putUsuario(book) {
          try {
              const response = await fetch(`http://localhost:8081/books/${book.id}`, {
                  method: 'PUT',
                  body: JSON.stringify(book),
                  headers: { 'Content-type': 'application/json; charset=UTF-8' },
              });
              const usuarioActualizado = await response.json();
              console.log("------")
              console.log(usuarioActualizado)
              this.books = this.books.map(u => (u.id === book.id ? usuarioActualizado : u));
          } catch (error) {
              console.error(error);
          }
      },
      async deleteUsuario(book) {
          try {
              await fetch(`http://localhost:8081/books/${book.id}`, {
                  method: "DELETE"
              });
              
              this.books= this.books.filter(u => u.id !== book.id);
          } catch (error) {
              console.error(error);
          }
      },
  },
  mounted() {
      this.getUsuarios();
  }
};
</script>