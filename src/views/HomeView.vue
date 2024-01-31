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
const errorApe: Ref<{ apellido: string}[]> = ref([]) 
const errorEdad: Ref<{ edad: string}[]> = ref([])
const errorGenero: Ref<{ genero: string}[]> = ref([])



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
  errorApe.value = []
  errorEdad.value = []
  errorGenero.value = []

  if (edad.value < 0 || edad.value > 60) {
    errorEdad.value.push({ edad: 'Edad no permitida' })    
  }
  if ((name.value.length < 5) || (name.value.length > 18 || name.value === '')) {
    errors.value.push({ name: 'Error de nombre' })    
  }
  if (name.value === apellido.value) {
    errorApe.value.push({ apellido: 'No puedes poner tu nombre en el apellido' })    
  }
  if (selectedGenero.value === 'Selecciona uno') {
    errorGenero.value.push({ genero: 'Selecciona un genero' })    
  }
  if (selectedGenero.value === 'Otro' && otroGenero.value === '') {
    errorGenero.value.push({ genero: 'Ingresa un genero' })    
  }
  if (selectedGenero.value === 'Otro' && otroGenero.value.length < 5) {
    errorGenero.value.push({ genero: 'Genero no permitido' })    
  }
  if (selectedGenero.value === 'Otro' && otroGenero.value.length > 18) {
    errorGenero.value.push({ genero: 'Genero no permitido' })    
  }
  if (selectedGenero.value === 'Otro' && otroGenero.value === genero.value) {
    errorGenero.value.push({ genero: 'Genero no permitido' })    
  }
}


</script>

<template>
  <main>
    <h3>Nombre</h3>
    <div>
      <input @input="validation" v-model="name" type="text" placeholder="Escribe tu nombre">
    </div>
    <span v-for="(err, index) in errors" :key="index">{{ err.name }}</span>
    <span></span>
    <h3>Apellido</h3>
    <div>
      <input @input="validation" v-model="apellido" type="text" placeholder="Escribe tu apellido">
    </div>
    <span v-for="(err, index) in errorApe" :key="index">{{ err.apellido }}</span>
    <h3>Edad</h3>
    <div>
      <input @input="validation" v-model="edad" type="number" placeholder="Escribe tu edad">
    </div>
    <span v-for="(err, index) in errorEdad" :key="index">{{ err.edad }}</span>
    <h3>Genero</h3>
    <div>
      <select @change="handleGeneroChange"  v-model="genero">
        <option v-for="(gen, index) in genre" :key="index">{{ gen }}</option>
      </select>
    </div>
    <div v-if="selectedGenero === 'Otro'">
      <input @input="validation" v-model="otroGenero" type="text">
      <span v-for="(err, index) in errorGenero" :key="index">{{ err.genero }}</span>
    </div>
  </main>
</template>

<style scoped@>
  main {
    max-width: 600px;
    margin: 0 auto;
    font-family: 'Arial', sans-serif;
  }

  h3 {
    margin-top: 20px;
  }

  input, select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 10px;
    box-sizing: border-box;
  }

  span {
    color: red;
    display: block;
    margin-bottom: 5px;
  }
  .error {
    color: red;
    margin-top: 5px;
  }
</style>
