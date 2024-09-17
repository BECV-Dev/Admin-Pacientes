<script setup>
  import { ref, reactive, onMounted, watch } from 'vue'
  import { uid } from 'uid'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

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

  onMounted(() => {
    const pacientesStorage = localStorage.getItem('pacientes')
    if(pacientesStorage){
      pacientes.value = JSON.parse(pacientesStorage)
    }
  })

  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  const actualizarPaciente = (id) => {
    const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
    Object.assign(paciente, pacienteEditar)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
  }

  const guardarPaciente = () => {
    if(paciente.id){
      const { id } = paciente
      const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id)
      pacientes.value[i] = {... paciente}
    }else {
        pacientes.value.push({
        ...paciente,
        id: uid()
      })
      guardarLocalStorage()
    }
    // console.log(pacientes.value[0])

    //Reiniciar el objeto
    Object.assign(paciente, {
      nombre: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: '',
      id: null
    })
  }

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
          v-model:nombre="paciente.nombre"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @guardar-paciente="guardarPaciente"
          :id="paciente.id"
       />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
        <div v-if="pacientes.length > 0">

          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>

          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />

        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
