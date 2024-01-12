<template>
    <div id="formulario-book">
        <form @submit.prevent="enviarFormulario">
            <div class="container">
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Isbn</label>
                            <input
                                ref="nombre"
                                v-model="book.isbn"
                                type="text"
                                class="form-control"
                                :class="{ 'is-invalid': procesando && isbnInvalido }"
                                @focus="resetEstado"
                                @keypress="resetEstado"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Title</label>
                            <input
                                v-model="book.title"
                                type="text"
                                :class="{ 'is-invalid': procesando && titleInvalido }"
                                class="form-control"
                                @focus="resetEstado"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Author</label>
                            <input
                                v-model="book.author"
                                type="text"
                                :class="{ 'is-invalid': procesando && authorInvalido }"
                                class="form-control"
                                @focus="resetEstado"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Price</label>
                            <input
                                v-model="book.price"
                                type="number"
                                step="0.01"
                                :class="{ 'is-invalid': procesando && priceInvalido }"
                                class="form-control"
                                @focus="resetEstado"
                            />
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <button class="btn btn-primary">Añadir libro</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="container">
                <div class="row">
                    <div class="col-md-12">
                        <div v-if="error && procesando" class="alert alert-danger" role="alert">
                            Debes rellenar todos los campos!
                        </div>
                        <div v-if="correcto" class="alert alert-success" role="alert">
                            El libro ha sido agregado correctamente!
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
    </template>
    
    <script>
    export default {
        name: 'formulario-book',
        data() {
          return {
                procesando: false,
                correcto: false,
                error: false,
                book: {
                    isbn: '',
                    title: '',
                    author: '',
                    price: '',
                }
          }
        },
        methods: {
            enviarFormulario() {
                this.procesando = true;
                this.resetEstado();
                
                // Comprobamos la presencia de errores
                if (this.isbnInvalidoInvalido || this.titleInvalido || this.authorInvalido || this.priceInvalido) {
                    this.error = true;
                    return;
                }
                this.$emit('crear-book', this.book);
                this.$refs.isbn.focus();
                this.error = false;
                this.correcto = true;
                this.procesando = false;
                // Restablecemos el valor de la variables
                this.book= {
                    isbn: '',
                    title: '',
                    author: '',
                    price: ''
                }
                console.log(this.book);
            },
            resetEstado() {
                this.correcto = false;
                this.error = false;
            }
        },
        computed: {
            isbnInvalido() {
                return this.book.isbn.length < 1;
            },
            titleInvalido() {
                return this.book.title.length < 1;
            },
            authorInvalido() {
                return this.book.author.length < 1;
            },
            priceInvalido() {
                return this.book.price < 1.0;
            },
        },
    }
    </script>
    
    <style scoped>
    form {
        margin-bottom: 2rem;
    }
    </style>