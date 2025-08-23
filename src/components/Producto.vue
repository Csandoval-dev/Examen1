<script setup>
defineEmits(['actualizar-producto', 'eliminar-producto'])

defineProps({
  producto: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="mx-5 my-10 bg-gray-100 shadow-md px-5 py-10 rounded-xl">
    <p class="font-bold mb-3 text-gray-700 uppercase">Código:
      <span class="font-normal normal-case">
        {{ producto.id }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Nombre del Producto:
      <span class="font-normal normal-case">
        {{ producto.nombreproducto }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Stock:
      <span class="font-normal normal-case">
        {{ producto.stock }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Precio:
      <span class="font-normal normal-case">
        Lps. {{ producto.precio }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Fecha de Vencimiento:
      <span class="font-normal normal-case">
        {{ producto.fechavencimiento }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Fecha de Creación:
      <span class="font-normal normal-case">
        {{ producto.fechacreacion }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">Proveedor:
      <span class="font-normal normal-case">
        {{ producto.proveedor }}
      </span>
    </p>

    <div class="grid md:grid-cols-2 gap-5 mt-10">
      <button
        type="button"
        class="block w-full py-2 px-10 bg-blue-800 hover:bg-blue-800
        text-white font-bold uppercase rounded-lg"
        @click="$emit('actualizar-producto', producto.id)"
      >
        Editar
      </button>

      <button
        type="button"
        class="block w-full py-2 px-10 bg-blue-800 hover:bg-blue-800
        text-white font-bold uppercase rounded-lg"
        @click="$emit('eliminar-producto', producto.id)"
      >
        Eliminar
      </button>
    </div>
  </div>
</template>
