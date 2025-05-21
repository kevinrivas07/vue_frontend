<template>
  <div class="writers-container">
    <h2>✍🏼 Writers</h2>
    <form @submit.prevent="addWriter">
      <div class="form-container">
        <input class="input" v-model="form.nombre" placeholder="Nombre" required />
        <input class="input" v-model="form.apellido" placeholder="Apellido" required />
        <input class="input" v-model="form.nacionalidad" placeholder="Nacionalidad" required />
        <input class="input" v-model.number="form.edad" type="number" placeholder="Edad" required />
        <button type="submit">Add Writer</button>
      </div>
    </form>

    <ul class="writers-list">
      <li v-for="w in writers" :key="w.id" class="writer-item">
        <div class="writer-info">
          {{ w.nombre }} {{ w.apellido }}
        </div>
        <div class="writer-details">
          {{ w.nacionalidad }}, {{ w.edad }} años
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { getWriters, createWriter, Writer } from '../services/api';
import '../styles/writers.css'; 

const writers = ref<Writer[]>([]);
const form = ref({ nombre: '', apellido: '', nacionalidad: '', edad: 0 });

const fetchWriters = async () => {
  writers.value = await getWriters();
};

const addWriter = async () => {
  await createWriter(form.value);
  form.value = { nombre: '', apellido: '', nacionalidad: '', edad: 0 };
  fetchWriters();
};

onMounted(fetchWriters);
</script>
