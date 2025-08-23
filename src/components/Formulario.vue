<script setup>
import { reactive, computed } from 'vue'
import Alerta from './Alerta.vue'

const alerta = reactive({
  tipo: '',
  mensaje: ''
})

const emit = defineEmits([
  'update:nombreproducto',
  'update:stock',
  'update:precio',
  'update:fechavencimiento',
  'update:fechacreacion',
  'update:proveedor',
  'guardar-producto'
])

const props = defineProps({
  codigo: {
    type: [String, null],
    required: true
  },
  nombreproducto: {
    type: String,
    required: true
  },
  stock: {
    type: String,
    required: true
  },
  precio: {
    type: String,
    required: true
  },
  fechavencimiento: {
    type: String,
    required: true
  },
  fechacreacion: {
    type: String,
    required: true
  },
  proveedor: {
    type: String,
    required: true
  }
})

const validar = () => {
  if (Object.values(props).includes('')) {
    alerta.mensaje = 'Todos los campos son obligatorios'
    alerta.tipo = 'error'
    return
  }
  emit('guardar-producto')
  alerta.mensaje = props.codigo ? ' Producto actualizado correctamente' : ' Producto almacenado correctamente'
  alerta.tipo = 'exito'

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: '',
      mensaje: ''
    })
  }, 3000)
}

const editando = computed(() => {
  return props.codigo
})
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Control de Productos</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Añade productos y
      <span class="black font-bold">Adminístralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" :alerta="alerta" />

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="nombreproducto" class="block text-gray-700 uppercase font-bold">
          Nombre del Producto
        </label>
        <input
          id="nombreproducto"
          type="text"
          placeholder="Ingrese el nombre del producto"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombreproducto"
          @input="$emit('update:nombreproducto', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="stock" class="block text-gray-700 uppercase font-bold">
          Stock
        </label>
        <input
          id="stock"
          type="number"
          placeholder="Cantidad en stock"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="stock"
          @input="$emit('update:stock', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="precio" class="block text-gray-700 uppercase font-bold">
          Precio
        </label>
        <input
          id="precio"
          type="number"
          step="0.01"
          placeholder="Precio del producto"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="precio"
          @input="$emit('update:precio', $event.target.value)"
        />
      </div>

      <!-- Fecha de Vencimiento -->
      <div class="mb-5">
        <label for="fechavencimiento" class="block text-gray-700 uppercase font-bold">
          Fecha de Vencimiento
        </label>
        <input
          id="fechavencimiento"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="fechavencimiento"
          @input="$emit('update:fechavencimiento', $event.target.value)"
        />
      </div>

      <!-- Fecha de Creación -->
      <div class="mb-5">
        <label for="fechacreacion" class="block text-gray-700 uppercase font-bold">
          Fecha de Creación
        </label>
        <input
          id="fechacreacion"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="fechacreacion"
          @input="$emit('update:fechacreacion', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="proveedor" class="block text-gray-700 uppercase font-bold">
          Proveedor
        </label>
        <input
          id="proveedor"
          type="text"
          placeholder="Proveedor del producto"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="proveedor"
          @input="$emit('update:proveedor', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        class="bg-blue-800 w-full p-3 text-white uppercase font-bold hover:bg-blue-800 cursor-pointer transition-colors"
        :value="[editando ? 'Guardar Cambios' : 'Registrar Producto']"
      />
    </form>
  </div>
</template>
