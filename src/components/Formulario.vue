<script setup>
    import { reactive, computed } from 'vue'
    import Alerta from './Alert.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })

    const validar = () => {
        if(Object.values(props).includes('')){
            showAlert(alerta, 'error', 'Todos los campos son obligatorios')
            // alerta.mensaje = 'Todos los campos son obligatorios'
            // alerta.tipo = 'error'
            return
        }

        if(editando.value){
            emit('guardar-paciente')
            showAlert(alerta, 'exito', 'Paciente actualizado correctamente')
        }else{
            emit('guardar-paciente')
            showAlert(alerta, 'exito', 'Paciente almacenado correctamente')
        }
        
        // emit('guardar-paciente')
        // showAlert(alerta, 'exito', 'Paciente almacenado correctamente')
        // alerta.mensaje = 'Paciente almacenado correctamente'
        // alerta.tipo = 'exito'
    }

    function showAlert(alerta, tipo, mensaje, timeout = 3000){
        alerta.tipo = tipo
        alerta.mensaje = mensaje
        
        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        }, timeout)
    }

    const editando = computed(() => {
        return props.id
    })

</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
        Añade Pacientes y
        <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>

    <Alerta
        v-if="alerta.mensaje"
        :alerta="alerta"
     />

    <form 
        class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
        @submit.prevent="validar"
    >

    <div class="mb-5">
        <label 
            for="mascota"
            class="text-gray-700 uppercase font-bold"
        >
            Nombre mascota
        </label>

        <input
            id="mascota"
            type="text"
            placeholder="Nombre de la mascota"
            class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
        />
    </div>

    <div class="mb-5">
        <label 
            for="propietario"
            class="text-gray-700 uppercase font-bold"
        >
            Nombre propietario
        </label>

        <input
            id="propietario"
            type="text"
            placeholder="Nombre del dueño"
            class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="propietario"
            @input="$emit('update:propietario', $event.target.value)"
        />
    </div>

    <div class="mb-5">
        <label 
            for="email"
            class="text-gray-700 uppercase font-bold"
        >
            Email
        </label>

        <input
            id="email" 
            type="email"
            placeholder="Email del dueño"
            class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="email"
            @input="$emit('update:email', $event.target.value)"
        />
    </div>

    <div class="mb-5">
        <label 
            for="alta"
            class="text-gray-700 uppercase font-bold"
        >
            Alta
        </label>

        <input
            id="alta" 
            type="date"
            class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="alta"
            @input="$emit('update:alta', $event.target.value)"
        />
    </div>

    <div class="mb-5">
        <label 
            for="sintomas"
            class="text-gray-700 uppercase font-bold"
        >
            Síntomas
        </label>

        <textarea
            id="sintomas" 
            placeholder="Describe los síntomas"
            class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
            :value="sintomas"
            @input="$emit('update:sintomas', $event.target.value)"
        />
    </div>

    <input 
        type="submit"
        :class="[editando ? 'bg-orange-600 hover:bg-orange-700' : 'bg-indigo-600 hover:bg-indigo-700']"
        class="w-full p-3 text-white uppercase font-bold cursor-pointer transition-colors"
        :value="[editando ? 'Actualizar paciente' : 'Registrar paciente']"
    />

    </form>

  </div>
</template>

