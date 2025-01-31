<script setup>
import { ref } from 'vue'

//goose img zone
const goose_mouth_image = ref('./gooseImages/goose_mouth_close.png')
const goose_mouth_image_open = ref('./gooseImages/goose_mouth_open.png');
const goose_mouth_image_close = ref('./gooseImages/goose_mouth_close.png');

//Change img zone
const changeImage = () => {
  if (goose_mouth_image_close.value === "./gooseImages/goose_mouth_close.png"||
  goose_mouth_image_open.value ==="./gooseImages/goose_mouth_open.png") {
    changeImagegoose()
  } else {
    changeImagegoose1()
  }
};

const mouthclose = () => {
  goose_mouth_image.value = goose_mouth_image_close.value
}

const mouthopen = () => {
  goose_mouth_image.value = goose_mouth_image_open.value
}

// เปลี่ยนภาพไปเซ็ตแบบที่1
const changeImagegoose = ()=>{
score_from_browser = score_count.value
goose_mouth_image_close.value = "./gooseImages/goose_mouth_close1.png";
goose_mouth_image_open.value = "./gooseImages/goose_mouth_open1.png";
goose_mouth_image.value = goose_mouth_image_close.value
score_count.value = score_from_browser1
}
 
// เปลี่ยนภาพกลับไปเซ็ตแบบปกติ
const changeImagegoose1 =()=>{
score_from_browser1 = score_count.value
goose_mouth_image_close.value = "./gooseImages/goose_mouth_close.png";
goose_mouth_image_open.value = "./gooseImages/goose_mouth_open.png"; 
goose_mouth_image.value = goose_mouth_image_close.value
score_count.value = score_from_browser
}
//score zone
// คะแนนgooseแบบปกติ
let score_from_browser = localStorage.getItem('score')
if (score_from_browser === null) {
  localStorage.setItem('score', 0)
  score_from_browser = 0
}
else {
  score_from_browser = Number.parseInt(score_from_browser)
}
// คะแนนgooseแบบที่1
let score_from_browser1 = localStorage.getItem('score1')
if (score_from_browser1 === null) {
  localStorage.setItem('score1', 0)
  score_from_browser1 = 0
}
else {
  score_from_browser1 = Number.parseInt(score_from_browser1)
}
const score_count = ref(score_from_browser) //reactive variable

function setScore(score) {
  score_count.value = score
  if (goose_mouth_image_close.value === "./gooseImages/goose_mouth_close.png"||
  goose_mouth_image_open.value ==="./gooseImages/goose_mouth_open.png") {
    localStorage.setItem('score', score)
  } else {
    localStorage.setItem('score1', score)
  }
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

//Random Event (+ - x ÷)
let displayshow = "display:block"
let display2show = "display:none"
let math1 = 0
let math2 = 0
let mathsum = 0
let mathran = Math.ceil(Math.random() * 20);
let mathran2 = Math.ceil(Math.random() * 4);
let MathOperation = "+"
let InputNumber = ref(0)
function MinigameMath(){
  if(score_count.value%13===mathran||score_count.value%17===mathran){
displayshow = "display:none"
display2show = "display:block"
math1 = 0
math2 = 0
mathran = Math.ceil(Math.random() * 10);
mathran2 = Math.ceil(Math.random() * 4)
do {
math1 = Math.ceil(Math.random() * 10000);
math2 = Math.ceil(Math.random() * 10000);
} while(math1<math2)
if(mathran2===0){
  MathOperation = "+"
  mathsum = math1+math2
} 
if(mathran2===1){
  MathOperation = "-"
  mathsum = math1-math2
} 
if(mathran2===2){
  MathOperation = "x"
  mathsum = math1*math2
}
if(mathran2===3){
  MathOperation = "÷"
  mathsum = (math1/math2).toFixed(2)
}

setTimeout(AnswerMinigame,20000)
}
}
function AnswerMinigame(){
  if(InputNumber.value==mathsum){
    score_count.value = score_count.value+(Math.floor(score_count.value/10))
    clearTimeout(setTimeout(AnswerMinigame,10000))
  }
  if(InputNumber.value!=mathsum) {
    score_count.value = Math.floor(score_count.value/2)
    clearTimeout(setTimeout(AnswerMinigame,10000))
  }
  math1 = 0
  math2 = 0
  displayshow = "display:block"
display2show = "display:none"
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

// reactive variable
const isOpen = ref(false)
//open & close MiniGameModal
const openMiniGameModal = () => {
  isOpen.value = true
}
const closeMiniGameModal = () => {
  isOpen.value = false
}

</script>

<template>
  <div>
  <!-- Background  -->
  <div v-bind:style="displayshow">
  <div v-bind:style="{ backgroundImage: `url(${background})` }"
    class="bg-no-repeat bg-cover flex items-center justify-center min-h-screen ">
    <div class="flex flex-col items-center">
      <h1 class="text-6xl font-bold text-gray-800 text-center bg-white rounded-md">
        PushGoose
      </h1>
      <!-- Count -->
      <h1 class="text-5xl font-bold text-gray-800 text-center bg-white ">
        {{ score_count }}
      </h1>
      <!-- Image -->
      <div class="w-4/5 max-w-xs sm:max-w-sm md:max-w-md lg:max-w-lg xl:max-w-xl aspect-square">
        <img class="w-full h-full cursor-pointer" v-bind:src="goose_mouth_image"
          @click="increaseCount(); RandomMultiple(); changeBackground(); MinigameMath()" @mousedown="mouthopen" @mouseup="mouthclose" />

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
          <button v-on:click="whenClicked"><img class="w-12 h-12 rounded-xl transition delay-150 duration-300 ease-in-out hover:-translate-y-1 
        hover:scale-110 cursor-pointer"
              v-bind:src="currentVolumeIcon"></button>
              <button @click="changeImage" class="px-6 py-3 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600 active:scale-95 shadow-lg transition-all 
              duration-300 cursor-pointer"> 
                Goose </button>
        </div>
      </div>
      <!-- mini game button -->
      <div class="absolute top-45 right-15 size-14">
        <button class="px-6 py-3 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600 transition delay-150 duration-300 ease-in-out hover:-translate-y-1 
        hover:scale-110 hover:bg-indigo-500 cursor-pointer" 
        @click="openMiniGameModal">Mini Game</button>
      </div>
      <!-- modal -->
      <div v-if="isOpen" class="fixed top-85 right-4 transform -translate-y-1/2 border-black border rounded-lg z-10 bg-white w-2xs max-w-lg">
      <!-- modal header -->
      <div v-if="isOpen" class="p-4 px-4 flex justify-between items-center border-black border-b text-xl font-bold">
        <div class="text-xl font-bold">Mini Game</div>
        <button @click="closeMiniGameModal" class="fond-bold text-2xl cursor-pointer transition-transform duration-0.2">&times;</button>  
      </div>
      <!-- modal body -->
      <div class="p-4 px-4">
        <!-- Math Quiz Button -->
        <button class="px-6 py-3 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600 transition delay-150 duration-300 ease-in-out hover:-translate-y-1 
        hover:scale-110 hover:bg-indigo-500 cursor-pointer">Math</button>
      </div>
    </div>
    <div class="fixed top-0 left-0 bottom-0 right-0 pointer-events-none"></div>
    </div>
  </div>
</div>
<div class="flex items-center justify-center min-h-screen bg-gray-100 m-0">
        <div class="bg-white p-8 rounded shadow-md w-full max-w-md">
            <h1 class="text-2xl font-bold mb-4">Number Quiz Game</h1>
            <p class="mb-4">Enter the result of the operation:</p>
            <div id="question" class="text-lg font-semibold mb-4">{{math1}} {{ MathOperation }} {{ math2 }} = {{ InputNumber }}</div>
            <input type="number" id="answer" class="border rounded w-full py-2 px-3 mb-4" placeholder="Your answer" v-model.numbers="InputNumber">
            <p class="mb-4 justify-center">- Answer Within 20 Second</p>
            <p class="mb-2 justify-center">- If Operation is divide. Answer With Two Decimal Place </p>
</div>
</div>
</div>
</template>

<style scoped></style>