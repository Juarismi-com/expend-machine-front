<template>
    <div class="max-w-xs mx-auto mt-10 p-4">
        <div class="text-center text-6xl pb-4 h-24">{{ input }}</div>

        <div class="grid grid-cols-3 gap-2">
        <button v-for="number in numbers" :key="number" class="p-8 text-4xl rounded-lg bg-gray-200 hover:bg-gray-300 transition" @click="appendNumber(number)">
            {{ number }}
        </button>

        <button class="p-4 text-xl rounded-lg bg-gray-200 hover:bg-gray-300 transition col-span-1 bg-red-500 text-white" @click="deleteLast">Borrar</button>
        <button class="p-4 text-xl rounded-lg bg-gray-200 hover:bg-gray-300 transition col-span-2 bg-green-500 text-white" @click="confirmInput">Confirmar</button>
        </div>
    </div>

    <hr class="mt-6">
    <div class="max-w-xs mx-auto mt-6 p-4" v-if="product != ''">
        <h3 class="text-xl">
            Producto: {{ product.nombre }}
            <br/> Precio: {{ product.precio }}
            <br/> Slot: {{ product.slot }}
        </h3>
    </div>
    <div class="max-w-xs mx-auto mt-6 p-4" v-if="error != ''">
        <h3 class="text-xl">{{  error  }}</h3>
    </div>
</template>
  
  <script setup>
  import { ref } from 'vue'
  import axios from 'axios'
  
  const input = ref('')
  const product = ref('')
  const error = ref('')
  const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  const appendNumber = (num) => {
    if (input.value.length < 2){
        product.value = ''
        error.value = ''
        input.value += num.toString()
    }
  }
  
  const deleteLast = () => {
    product.value = ''
    error.value = ''
    input.value = input.value.slice(0, -1) 
  }
  
  const confirmInput = async () => {
    const lastInputValue = input.value;

    try {
        await axios.post(`http://192.168.100.15:3000/pos/venta-qr`, {
            facturaNro: 99,
            monto: 100,
            montoVuelto:0
        });

        if (lastInputValue == '')
             error.value = 'Por favor seleccione un slot'
        else {
            error.value = ''
            const response = await axios.get(`http://192.168.100.14:5001/signal/${lastInputValue}`);
            product.value = response?.data
            input.value = ''
        }
    } catch (e) {
        error.value = 'No se encontro el producto en el slot ' + lastInputValue
        input.value = ''
        console.log(e);
    }
  }
  </script>
  
  
  