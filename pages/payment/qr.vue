<template>
    <div class="flex flex-col items-center justify-center min-h-screen p-4">
      <div class="bg-white p-6 rounded-xl shadow-lg w-full max-w-md text-center">
        <h1 class="text-2xl font-semibold mb-4">Paga con QR</h1><br><br><br><br><br><br>
  
        <!--QRCodeVue3
          :value="paymentUrl"
          :size="200"
          :margin="2"
          class="mx-auto mb-4"
        /-->
        <div class="justify-center">
          <img class="absolute inset-0 m-auto w-48 h-48" :src="`https://api.qrserver.com/v1/create-qr-code/?data=${paymentUrl}&size=200x200`" />
        </div>
  
        <br>
        <br>
        <br>
        <p class="text-gray-600 mb-2">Escanea el código con tu app de pagos.</p>
        <p class="text-sm text-gray-400">ID de transacción: {{ transactionId }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  //import QRCodeVue3 from 'qrcode.vue'
  
  // Simulación de datos de pago
  const transactionId = 'TXN-' + Math.floor(Math.random() * 1000000)
  const paymentUrl = `https://miapp.com/pago/${transactionId}`
  </script>
  