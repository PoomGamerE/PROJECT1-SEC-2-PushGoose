<script setup>
import { ref } from 'vue'

//goose img zone
const goose_mouth_image = ref('./gooseImages/goose_mouth_close.png')

//score zone
const score_count = ref(0) //reactive variable

const increaseCount = () => {
  goose_mouth_image.value = './gooseImages/goose_mouth_open.png'
  score_count.value++
  //goose_mouth_image.value = './gooseImages/goose_mouth_close.png'
}

</script>

<template>
  <div class="bg-orange-100 flex items-center justify-center min-h-screen">
    <div class="flex flex-col items-center">
      <h1 class="text-6xl font-bold text-gray-800 text-center">
        PushGoose
      </h1>
      <!-- Count -->
      <h1 class="text-5xl font-bold text-gray-800 text-center">
        {{ score_count }}
      </h1>
      <!-- Image -->
      <div class="w-4/5 max-w-xs sm:max-w-sm md:max-w-md lg:max-w-lg xl:max-w-xl aspect-square">
        <img class="w-full h-full" v-bind:src="goose_mouth_image" v-on:click="increaseCount" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>