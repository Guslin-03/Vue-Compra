<template>
  <div class="app">
    <header class="header">
      <h1>{{ nombreApp }}</h1>
    </header>
    <main class="main">
      <div class="input-container">
        <input
          type="text"
          class="product-input"
          placeholder="Producto"
          v-model="nombreNuevoArticulo"
          @keyup.enter="agregarArticulo"
        />
        <button class="add-button" @click="agregarArticulo()">Añadir</button>
      </div>
      <ul class="product-list">
        <li class="product-item" v-for="(articulo, index) in lista" :key="index">
          <div class="product-quantity">
            <input type="number" class="quantity-input" v-model.number="articulo.cantidad" />
          </div>
          <a href="#" class="product-name">{{ articulo.nombre }}</a>
          <div class="product-actions">
            <button class="increase-button" @click="sumarTotal(1), articulo.cantidad++">+</button>
            <button class="decrease-button" @click="restarTotal(1), articulo.cantidad > 0 && articulo.cantidad--">
              -
            </button>
            <button class="delete-button" @click="eliminarArticulo(index)">Borrar</button>
          </div>
        </li>
      </ul>
      <div v-if="total > 0">
          <h3>Cantidad Total: {{ total }}</h3>
      </div>
      <div>
        <h4>Artículos del Carrito</h4>
        <ul class="product-list">
        <li class="product-item" v-for="(articulo, index) in lista" :key="index">
          <div class="product-quantity">
            <input type="number" class="quantity-input" v-model.number="articulo.cantidad" />
          </div>
          <p href="#" class="product-name">{{ articulo.nombre }} -> {{ articulo.cantidad }}</p>
          <h4>Cantidad Total: {{ totalCarrito }}</h4>
        </li>
      </ul>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const nombreApp = ref("Lista de la compra");
    const nombreNuevoArticulo = ref("");
    const lista = ref([]);
    var total = ref(0);
    var totalCarrito = ref(0);

    const agregarArticulo = () => {
      if (nombreNuevoArticulo.value.trim() === "") {
        alert("El campo de producto está vacío.");
        return;
      }

      // Pedir la cantidad del artículo al usuario
      const cantidad = prompt("Ingrese la cantidad del artículo:");
      const cantidadNumerica = parseInt(cantidad, 10);

      if (isNaN(cantidadNumerica) || cantidadNumerica <= 0) {
        alert("Por favor, ingrese un número válido mayor que 0.");
        return;
      }

      // Crear el objeto del artículo
      const nuevoArticulo = {
        nombre: nombreNuevoArticulo.value,
        cantidad: cantidadNumerica,
      };

      sumarTotal(cantidadNumerica)

      // Añadir el artículo al array lista
      lista.value.push(nuevoArticulo);

      // Limpiar el campo de texto
      nombreNuevoArticulo.value = "";
    };

    const eliminarArticulo = (index) => {
      restarTotal(lista.value.at(index).cantidad);
      lista.value.splice(index, 1);
    };

    function sumarTotal (cantidad) {
      total.value += cantidad;
    }

    function restarTotal (cantidad) {
      total.value -= cantidad;
    }

    return {
      nombreApp,
      nombreNuevoArticulo,
      lista,
      total,
      agregarArticulo,
      eliminarArticulo,
      sumarTotal,
      restarTotal
    };
  },
};
</script>

<style scoped>
/* Estilos generales */
.app {
  font-family: Arial, sans-serif;
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
}

/* Estilo del header */
.header {
  text-align: center;
  margin-bottom: 20px;
}

.header h1 {
  font-size: 2rem;
  color: #333;
}

/* Contenedor del input */
.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.product-input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.add-button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #0056b3;
}

/* Lista de productos */
.product-list {
  list-style: none;
  padding: 0;
}

.product-item {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.quantity-input {
  width: 50px;
  padding: 4px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.product-name {
  flex: 1;
  text-decoration: none;
  color: #007bff;
}

.product-name:hover {
  text-decoration: underline;
}

.product-actions {
  display: flex;
  gap: 5px;
}

button {
  padding: 4px 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #f0f0f0;
}

.delete-button {
  background-color: #dc3545;
  color: white;
  border: none;
}

.delete-button:hover {
  background-color: #b02a37;
}
</style>
