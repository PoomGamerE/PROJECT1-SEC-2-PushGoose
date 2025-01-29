<script setup>
import { ref } from 'vue'

//goose img zone
const goose_mouth_image = ref('./gooseImages/goose_mouth_close.png')

//Change img zone
const changeImage = () => {
  if (goose_mouth_image.value === './gooseImages/goose_mouth_close.png') {
    goose_mouth_image.value = './gooseImages/goose_mouth_close1.png'
  } else {
    goose_mouth_image.value = './gooseImages/goose_mouth_close.png'
  }
}

const mouthclose = () => {
  goose_mouth_image.value = './gooseImages/goose_mouth_close.png'
}

const mouthopen = () => {
  goose_mouth_image.value = './gooseImages/goose_mouth_open.png'
}

//score zone
let score_from_browser = localStorage.getItem('score')
if (score_from_browser === null) {
  localStorage.setItem('score', 0)
  score_from_browser = 0
}
else {
  score_from_browser = Number.parseInt(score_from_browser)
}
const score_count = ref(score_from_browser) //reactive variable

function setScore(score) {
  score_count.value = score
  localStorage.setItem('score', score)
}

//for Multiple
let nclick = 0
let PresentMultiple = ref(1)
let MColor = "color:black"

const increaseCount = () => {
  setScore(score_count.value + (1 * PresentMultiple.value))
}

const volumeOn = './volumeOn_Off/volume_on.png' //volume-on.png
const volumeOff = './volumeOn_Off/volume_off.png' //volume-off.png
const currentVolumeIcon = ref(volumeOn) //reactive variable

const whenClicked = () => {
  currentVolumeIcon.value =
    currentVolumeIcon.value === volumeOn ? volumeOff : volumeOn;
  // if the currentVolumeIcon.value = volumeOn is TRUE the value is going to be volumeOff
  // if not it's volumeOn
}

//Random Multiple 
function RandomMultiple() {
  nclick++ //นับว่าคลิกไปกี่ครั้งแล้วหลังจากไม่ได้ตัวคูณ
  const ran = Math.ceil(Math.random() * 30);
  if (ran - nclick > 0) { //ยิ่งกดไม่ติดเยอะ โอกาศยิ่งเพิ่มขึ้นการันตีอยู่ที่30ครั้ง
    PresentMultiple.value = 1 //รีเซ็ตตัวคูณ
    MColor = "color:black"
    return PresentMultiple.value
  } else if (ran - nclick <= 0) {
    PresentMultiple.value = Math.ceil(Math.random() * 10) //สุ่มตัวคูณ1-10
    nclick = 0 //รีเซ็ตค่าคลิก
    if (PresentMultiple.value > 3) {
      MColor = "color:green"
      if (PresentMultiple.value > 6) {
        MColor = "color:blue"
        if (PresentMultiple.value > 9) {
          MColor = "color:red"
        }
      }
    }
    return PresentMultiple.value
  }
}

//Change Background 
const bgWindowXP = './backgroundImages/WindowsXp.jpg'
const bgFlame = './backgroundImages/Fireflam.jpg'
const bgBrain = './backgroundImages/Brain.jpg'

const background = ref(bgWindowXP) //Original Background

function changeBackground() {
  if (score_count.value <= 200) {
    background.value = bgWindowXP
  } else if (score_count.value > 200 && score_count.value <= 1000) {
    background.value = bgFlame
  } else {
    background.value = bgBrain
  }
}
</script>

<template>
  <!-- Background  -->
  <div v-bind:style="{ backgroundImage: `url(${background})` }"
    class="bg-no-repeat bg-cover flex items-center justify-center min-h-screen ">
    <div class="flex flex-col items-center">
      <button @click="changeImage" class="bg-white underline"> เปลี่ยนภาพ </button>
      <h1 class="text-6xl font-bold text-gray-800 text-center bg-white rounded-md">
        PushGoose
      </h1>
      <!-- Count -->
      <h1 class="text-5xl font-bold text-gray-800 text-center bg-white ">
        {{ score_count }}
      </h1>
      <!-- Image -->
      <div class="w-4/5 max-w-xs sm:max-w-sm md:max-w-md lg:max-w-lg xl:max-w-xl aspect-square">
        <img class="w-full h-full" v-bind:src="goose_mouth_image"
          @click="increaseCount(); RandomMultiple(); changeBackground()" @mousedown="mouthopen" @mouseup="mouthclose" />

      </div>
      <!-- Multiple -->
      <div>
        <h2 class="text-10 italic font-bold  text-center bg-white " v-bind:style="MColor"
          v-text="`X` + PresentMultiple">
        </h2>
      </div>
      <!-- VolumeOn-Off -->
      <div>
        <div class="absolute top-15 right-15 size-14">
          <button v-on:click="whenClicked"><img class="w-12 h-12 bg-white rounded-xl"
              v-bind:src="currentVolumeIcon"></button>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped></style>