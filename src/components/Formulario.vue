<script setup>
import { reactive, computed } from "vue";
import Alerta from "./Alerta.vue";

const emit = defineEmits([
'update:nombre', 'update:propietario', 
'update:email', 'update:alta', 
'update:sintomas', 'agregar-paciente',
'delete-id'])

const alerta = reactive({
  tipo: '',
  mensaje: ''
})

const props = defineProps({
  nombre:{
    type: String,
    required: true
  },
  propietario:{
    type: String,
    required: true
  },
  email:{
    type: String,
    required: true
  },
  alta:{
    type: String,
    required: true
  },
  sintomas:{
    type: String,
    required: true
  },
  id:{
    type: [String, null],
    required: true  
  }
})

const validar = () => {
  if(Object.values(props).includes('')){
    alerta.mensaje = 'Todos los campos son obligatorios'
    alerta.tipo = 'error'
    setTimeout(() => alerta.mensaje = '', 2500)
    return
  }else if(props.id){
    alerta.mensaje = 'Actualizado correctamente'
  }else{
    alerta.mensaje = 'Agregado correctamente'
  }
  alerta.tipo = 'exito'
  setTimeout(() => alerta.mensaje = '', 2500)
  emit('agregar-paciente')
}

const editando = computed(() => (props.id) ? 'Actualizar paciente' : 'Registrar paciente')

const cancelar = () => {
  emit('update:nombre', '')
  emit('update:propietario', '')
  emit('update:email', '')
  emit('update:alta', '')
  emit('update:sintomas', '')
  emit('delete-id')
}

</script>

<template>
  <div class="md:w-1/2 ">
    <h2 class="sm:text-3xl font-black text-center ">
        Seguimiento Pacientes
    </h2>
    <p class="text-lg mt-5 text-center mb-10">
        Añade Pacientes y
        <span
        class="text-indigo-600 font-bold"
        >adminístralos
        </span>
    </p>
    <form 
    class="bg-white rounded-lg shadow-md py-10 px-5 mb-10"
    @submit.prevent="validar">
      <div class="mb-5">
        <label for="mascota" 
        class="block text-gray-700 uppercase font-bold">Nombre mascota</label>
        <input type="text" 
        placeholder="Nombre de la mascota"
        id="mascota"
        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
        :value="nombre"
        @input="$emit('update:nombre', $event.target.value)">
      </div>
      <div class="mb-5">
        <label for="propietario" 
        class="block text-gray-700 uppercase font-bold">Nombre propietario</label>
        <input type="text" 
        placeholder="Nombre del propietario"
        id="propietario"
        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
        :value="propietario"
        @input="$emit('update:propietario', $event.target.value)">
      </div>
      <div class="mb-5">
        <label for="email" 
        class="block text-gray-700 uppercase font-bold">E-mail propietario</label>
        <input type="email" 
        placeholder="E-mail"
        id="email"
        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
        :value="email"
        @input="$emit('update:email', $event.target.value)">
      </div>
      <div class="mb-5">
        <label for="alta" 
        class="block text-gray-700 uppercase font-bold">Alta</label>
        <input type="date" 
        id="alta"
        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
        :value="alta"
        @input="$emit('update:alta', $event.target.value)">
      </div>
      <div class="mb-5">
        <label for="sintomas" 
        class="block text-gray-700 uppercase font-bold">Sintomas</label>
        <textarea
        placeholder="Describe los síntomas"
        id="sintomas"
        class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
        :value="sintomas"
        @input="$emit('update:sintomas', $event.target.value)"/>
      </div>
      <div class="lg:flex">
        <input type="submit" class="w-full bg-indigo-600 p-2 text-white uppercase
        font-bold hover:bg-indigo-700 cursor-pointer transition-colors rounded-md mb-5 lg:mb-0"
        :value="editando">
        <button v-if="id" type="button" class="w-full bg-gray-600 p-2 text-white uppercase
        font-bold hover:bg-gray-700 cursor-pointer transition-colors rounded-md lg:ml-2"
        @click="cancelar"
        >Cancelar</button>
      </div>
      <Alerta v-if="alerta.mensaje" :alerta="alerta" />
    </form>
  </div>
</template>
