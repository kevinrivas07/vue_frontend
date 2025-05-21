<template>
  <div class="books-container">
    <h2>📖 Books</h2>
    <form @submit.prevent="addBook">
      <div class="form-container">
        <input class="input" v-model="form.nombre" placeholder="Título del libro" required />
        <input class="input" v-model="form.genero" placeholder="Género literario" required />
        <input class="input" v-model.number="form.añoPublicacion" type="number" placeholder="Año de publicación" required />
        <select class="input" v-model.number="form.author.id" required>
          <option disabled value="0">Selecciona el autor</option>
          <option v-for="w in writers" :key="w.id" :value="w.id">{{ w.nombre }} {{ w.apellido }}</option>
        </select>
        <button type="submit">Agregar Libro</button>
      </div>
    </form>

    <ul class="books-list">
      <li v-for="b in books" :key="b.id" class="book-item">
        <div class="book-title">{{ b.nombre }}</div>
        <div class="book-author">por {{ b.author.nombre }} {{ b.author.apellido }}</div>
        <div class="book-details">
          <span class="book-year">{{ b.añoPublicacion }}</span>
          <span class="book-genre">{{ b.genero }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { getBooks, createBook, getWriters, Book, Writer } from '../services/api';
import '../styles/books.css';

const books = ref<Book[]>([]);
const writers = ref<Writer[]>([]);
const form = ref({ nombre: '', genero: '', añoPublicacion: 0, author: { id: 0 } });

const fetchWriters = async () => {
  writers.value = await getWriters();
};

const fetchBooks = async () => {
  books.value = await getBooks();
};

const addBook = async () => {
  await createBook(form.value);
  form.value = { nombre: '', genero: '', añoPublicacion: 0, author: { id: 0 } };
  fetchBooks();
};

onMounted(() => {
  fetchWriters();
  fetchBooks();
});
</script>
