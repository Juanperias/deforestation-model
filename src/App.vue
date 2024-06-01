<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as tf from '@tensorflow/tfjs'

const year = ref(0)
const predictionResult = ref(0)

async function makePrediction() {
  const loadedModel = await tf.loadLayersModel('http://localhost:5173/model.json')

  const tensor = tf.tensor1d([parseInt(year.value.toString())])
  const prediction = loadedModel.predict(tensor)
  predictionResult.value = parseInt(
    prediction
      .toString()
      .replaceAll('[', ' ')
      .replace('Tensor', ' ')
      .replaceAll(']', ' ')
      .replaceAll(',', ' ')
      .trim()
  )
}
</script>

<template>
  <h1 class="text-white">deforestation prediction model</h1>
  <input type="text" v-model="year" placeholder="type a year" />
  <button class="text-white ml-2 p-3 bg-blue-600 rounded" @click="makePrediction">
    Make Prediction
  </button>
  <p class="text-white">Result: {{ predictionResult }} millions of hectares</p>
</template>
