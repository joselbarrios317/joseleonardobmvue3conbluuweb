<script setup>
import { ref, computed } from 'vue'

const name = 'Vue Dinámico';

const counter = ref(0);
const listNumbers = ref([]);

const decrease = () => counter.value--;
const increment = () => counter.value++;
const restart = () => counter.value = 0;

const add = () => {
  const number = {number: counter.value, hover:false };
  listNumbers.value.push(number)
};
const deleteNumber = () => {
  listNumbers.value = listNumbers.value.filter(number => number.number != counter.value);
};

const classCounter = computed(() => {
  const classCounter = counter.value > 0 ? 'positiveValue' : counter.value < 0 ? 'negativeValue' : 'zero';
  return classCounter
})
const existNumber = computed(() => {
  const numbers = listNumbers.value.map( n => n.number);
  return numbers.includes(counter.value);

});

const selectNumber = (number) => counter.value = number.number;

const toggleHover = (number) => {
  number.hover = !number.hover;
}

const clearNumbers = () => {
  listNumbers.value = [];
};

</script>

<template>
  <h1 class="ps-4"> Hola {{ name.toUpperCase() }} </h1>
  <div class="d-flex align-items-center flex-column">
    <div style="max-width: 380px;">
      <div class="container text-center mt-3">
        <div class="btn-group"></div>
        <button @click="restart" class="btn btn-warning"> Reiniciar </button>
      </div>
      <div class="container text-center mt-3">
        <div class="btn-group">
          <button @click="decrease" class="btn btn-danger"> Disminuir </button>
          <h2 :class="classCounter" style="min-width: 100px;">
            {{ counter }}
          </h2>
          <button @click="increment" class="btn btn-success"> Aumentar </button>
        </div>
      </div>
      <div class="container text-center mt-3">
        <div class="btn-group">
          <button @click="deleteNumber" class="btn btn-secondary" :disabled="!existNumber">
            Eliminar
          </button>
          <button @click="clearNumbers" class="btn btn-warning" :disabled="listNumbers.length < 1">
            Limpiar Numeros
          </button>
          <button @click="add" class="btn btn-primary" :disabled="existNumber">
            Agregar
          </button>
          
        </div>
      </div>
    </div>
    <div style="width: 600px; max-width: 600px;">
      <div class="container text-center mt-3">
        <h2> Lista de Números <b> [{{ listNumbers.length }}] </b> </h2>
        <ul class="list-group">
          <li 
            v-for="(number, index) of listNumbers"
            :key="index"
            class="list-group-item"
            @click="selectNumber(number)"
            :class="{'bg-success':number.number === counter, 'bg-primary': !(number.number === counter) && number.hover === true }"
            @mouseover ="toggleHover(number)"
            @mouseleave = "toggleHover(number)"
          >
            {{ number.number }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  color: red;
}


.container_counter_header {
  padding-bottom: 10px;
  padding-left: 20px;
  padding-right: 20px;
}

.container_counter {
  padding-left: 20px;
  padding-right: 20px;
  display: flex;
  justify-content: space-between;
}

.container_counter_footer {
  padding-top: 10px;
}

.positiveValue {
  color: green;
  font-weight: bold;
}

.negativeValue {
  color: red;
  font-weight: bold;
}

.zero {
  color: peru;
  font-weight: bold;
}
</style>