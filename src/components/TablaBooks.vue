<template>
    <div id="tabla-books">
        <div v-if="!books.length" class="alert alert-info" role="alert">
            No se han agregado libros
        </div>
        <table class="table">
            <thead>
                <tr>
                    <!--<th>Id</th> -->
                    <th>Isbn</th>
                    <th>Title</th>
                    <th>Author</th>
                    <th>Price</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="book in books" :key="book.id">
                    <!--
                    <td v-if="editando === usuario.id">
                        <input type="text" class="form-control" v-model="usuario.id" />
                    </td>
                    <td v-else>
                        {{ usuario.id}} 
                    </td>
                    -->
                    <td v-if="editando === book.id">
                        <input type="text" class="form-control" v-model="book.isbn" />
                    </td>
                    <td v-else>
                        {{ book.isbn }}
                    </td>
                    <td v-if="editando === book.id">
                        <input type="text" class="form-control" v-model="book.title" />
                    </td>
                    <td v-else>
                        {{ book.title }}
                    </td>
                    <td v-if="editando === book.id">
                        <input type="text" class="form-control" v-model="book.author" />
                    </td>
                    <td v-else>
                        {{ book.author }}
                    </td>
                    <td v-if="editando === book.id">
                        <input type="number" step="0.01" class="form-control" v-model="book.price" />
                    </td>
                    <td v-else>
                        {{ book.price }}
                    </td>
                    <td v-if="editando === book.id">
                        <button class="btn btn-success" @click="guardarUsuario(book)">💾 Guardar</button>
                        <!--<button class="btn btn-success" @click="$emit('actualizar-usuario', usuario)">💾 Guardar</button>-->
                        <button class="btn btn-secondary ml-2" @click="cancelarEdicion(book)">❌ Cancelar</button>
                    </td>
                    <td v-else>
                        <button class="btn btn-info" @click="editarUsuario(book)">✏️ Editar</button>
                        <button class="btn btn-danger ml-2" @click="$emit('eliminar-usuario', book)">🗑️ Eliminar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    </template>
    <script>
      export default {
        name: 'tabla-books',
        props: {
           books: Array,
        },
        data() {
            return {
                editando: null,
            }
        },
        methods: {
            editarUsuario(book) {
                this.usuarioEditado = Object.assign({}, book);
                this.editando = book.id;
            },
            guardarUsuario(book) {
                if (!book.isbn.length || !book.title.length || !book.author.length || !book.price) {
                    return;
                }
                this.$emit('actualizar-usuario', book);
                this.editando = null;
            },
            cancelarEdicion(book) {
                Object.assign(book, this.usuarioEditado);
                this.editando = null;
            }
        }
      }
    </script>