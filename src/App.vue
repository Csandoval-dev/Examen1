<script setup>
import { ref, reactive, watch, onMounted } from 'vue'
import { uid } from 'uid'
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import Producto from './components/Producto.vue'

const productos = ref([])

const producto = reactive({
  codigo: null,
  nombreproducto: '',
  stock: '',
  precio: '',
  fechavencimiento: '',
  fechacreacion: '',
  proveedor: ''
})

watch(productos, () => {
  guardarLocalStorage()
}, { deep: true })

const guardarLocalStorage = () => {
  localStorage.setItem('productos', JSON.stringify(productos.value))
}

onMounted(() => {
  const productosStorage = localStorage.getItem('productos')
  if (productosStorage) {
    productos.value = JSON.parse(productosStorage)
  }
})

const guardarProducto = () => {
  if (
    !producto.nombreproducto ||
    !producto.stock ||
    !producto.precio ||
    !producto.fechavencimiento ||
    !producto.fechacreacion ||
    !producto.proveedor
  ) {
    alert(' Todos los campos son obligatorios')
    return
  }

  if (producto.codigo) {
    const { codigo } = producto
    const i = productos.value.findIndex(p => p.codigo === codigo)
    productos.value[i] = { ...producto }
    alert(' Producto actualizado correctamente')
  } else {
    productos.value.push({
      ...producto,
      codigo: uid()
    })
    alert(' Producto almacenado correctamente')
  }

  producto.nombreproducto = ''
  producto.stock = ''
  producto.precio = ''
  producto.fechavencimiento = ''
  producto.fechacreacion = ''
  producto.proveedor = ''
  producto.codigo = null
}

const actualizarProducto = (codigo) => {
  const productoEditar = productos.value.find(p => p.codigo === codigo)
  Object.assign(producto, productoEditar)
  
}

const eliminarProducto = (codigo) => {
  const confirmar = confirm('¿Deseas eliminar este producto?')
  if (confirmar) {
    productos.value = productos.value.filter(p => p.codigo !== codigo)
    alert(' Producto eliminado correctamente')
  }
}
</script>

<template>
  <div class="container mx-auto mt-10">
    <Header />

    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombreproducto="producto.nombreproducto"
        v-model:stock="producto.stock"
        v-model:precio="producto.precio"
        v-model:fechavencimiento="producto.fechavencimiento"
        v-model:fechacreacion="producto.fechacreacion"
        v-model:proveedor="producto.proveedor"
        @guardar-producto="guardarProducto"
        :codigo="producto.codigo"
      />

      <div class="md:w-1/2 md:h-screen overflow-scroll">
        <h3 class="font-black text-3xl text-center">Inventario de Productos</h3>

        <div v-if="productos.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="black">Productos</span>
          </p>

          <Producto
            v-for="prod in productos"
            :key="prod.codigo"
            :producto="prod"
            @actualizar-producto="actualizarProducto"
            @eliminar-producto="eliminarProducto"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay productos</p>
      </div>
    </div>
  </div>
</template>
