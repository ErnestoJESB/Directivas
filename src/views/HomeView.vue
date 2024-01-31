<script setup lang="ts">
import { ref } from 'vue'
import type {Ref} from 'vue'

const name:Ref<string> = ref('')
const apellido:Ref<string> = ref('')
const edad: Ref<number> = ref(0)
const genero: Ref<string> = ref('Selecciona uno')
const genre: Ref<string[]> = ref(['Selecciona uno', 'Masculino', 'Femenino', 'Otro'])
const selectedGenero: Ref<string> = ref('')
const otroGenero: Ref<string> = ref('')
const errors: Ref<{ name: string}[]> = ref([]) 


/* Realizar un formulario donde se valide lo siguiente:
 nombre -> Validar mínimo 5 caracteres máximo 18
 Apellido -> Validar que no sea igual al nombre
 edad -> Validar que sea mayor a 0 y menor a 60
  Genero -> Masculino y femenino, otro -> al seleccionar otro, se debe mostrar un input para ingresar el genero
*/

 
const handleGeneroChange = () => {
  selectedGenero.value = genero.value
}

const validation = () => {
  errors.value = []

  if ((name.value.length < 5) || (name.value.length > 18)) {
    errors.value.push({ name: 'Error de nombre' })    
  }
  if (name.value === apellido.value) {
    errors.value.push({ name: 'No puedes poner tu nombre' })    
  }
  if (edad.value < 0 || edad.value > 60) {
    errors.value.push({ name: 'Edad no permitida' })    
  }
}


</script>

<template>
  <main>
    <h3>Nombre</h3>
    <div>
      <input v-model="name" type="text" placeholder="Escribe tu nombre">
    </div>
    <span></span>
    <h3>Apellido</h3>
    <div>
      <input v-model="apellido" type="text" placeholder="Escribe tu apellido">
    </div>
    <h3>Edad</h3>
    <div>
      <input v-model="edad" type="number" placeholder="Escribe tu edad">
    </div>
    <h3>Genero</h3>
    <div>
      <select @change="handleGeneroChange"  v-model="genero">
        <option v-for="(gen, index) in genre" :key="index">{{ gen }}</option>
      </select>
    </div>
    <div v-if="selectedGenero === 'Otro'">
      <input v-model="otroGenero" type="text">
    </div>
    <button @click="validation">Validar</button>
    
    <h3>Errors</h3>
    <span v-for="(err, index) in errors" :key="index">{{ err.name }}</span>
    

    <h3>Tus Datos son los siguientes</h3> {{ name }} {{ apellido }} {{ edad }} {{ selectedGenero }} {{ otroGenero }}
  </main>
</template>

<style scoped@>
.error{
  color: white;
  background-color: red;
  border: 1px solid black;
}
</style>