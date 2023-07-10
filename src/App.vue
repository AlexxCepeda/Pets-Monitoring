<script setup>
import {ref, reactive, watch, onMounted} from 'vue'
import {uid} from 'uid'
import Formulario from "./components/Formulario.vue"
import Header from "./components/Header.vue"
import Paciente from './components/Paciente.vue';

const pacientes = ref([])

const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''
})

watch(pacientes, () => {
  guardarLocalStorage()
}, {
  deep: true
})


const agregarPaciente = () => {
  if(paciente.id){
    const {id} = paciente
    const i = pacientes.value.findIndex(pacienteState => pacienteState.id === id)
    pacientes.value[i] = {...paciente}
  }else{
    pacientes.value.push({
      ...paciente, 
      id: uid()
    });
  }
  paciente.nombre = ''
  paciente.propietario = ''
  paciente.email = ''
  paciente.alta = ''
  paciente.sintomas = ''
  paciente.id = null
}

const editarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
  Object.assign(paciente, pacienteEditar)
}

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
}

const deleteId = () => {
  paciente.id = null
}

const guardarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
}
onMounted(() => {
  const pacientesStorage = localStorage.getItem('pacientes')
  if(pacientesStorage){
    pacientes.value = JSON.parse(pacientesStorage)
  }
})
</script>

<template>
  <div class="container mx-auto mt-10 sm:mt-20">
    <Header />
    <div class="w-11/12 mx-auto mt-8 sm:mt-12 md:flex" >
      <Formulario 
      v-model:nombre="paciente.nombre"
      v-model:propietario="paciente.propietario"
      v-model:email="paciente.email"
      v-model:alta="paciente.alta"
      v-model:sintomas="paciente.sintomas"
      @agregar-paciente="agregarPaciente"
      @delete-id="deleteId"
      :id="paciente.id"

      />

      <div class="md:w-1/2">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
        <p class="text-lg mt-5 text-center mb-10">
        Información de
        <span
        class="text-indigo-600 font-bold"
        >Pacientes
        </span>
    </p>
        <div 
          v-if="pacientes.length > 0" 
          class="md:h-screen overflow-y-scroll" >
            <Paciente 
              v-for="(paciente,index) in pacientes" 
              :key="index" 
              :paciente="paciente" 
              @editar-paciente="editarPaciente"
              @eliminar-paciente="eliminarPaciente"
              class="first-of-type:my-0"/>
        </div>
        <p v-else class="mt-20 text-2xl text-center font-bold">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
