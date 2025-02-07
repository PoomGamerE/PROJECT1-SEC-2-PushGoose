<script setup>
import { ref, watch } from 'vue'

//goose img zone
const goose_mouth_image = ref('./gooseImages/goose_mouth_close.png')
const goose_mouth_image_open = ref('./gooseImages/goose_mouth_open.png');
const goose_mouth_image_close = ref('./gooseImages/goose_mouth_close.png');

//Change img zone
const changeImage = (event) => {
  if (event.target.value==="BadGoose") {
    changeImagegoose()
  } else if (event.target.value === "Goose")
    changeImagegoose1()
  }

const mouthclose = () => {
  goose_mouth_image.value = goose_mouth_image_close.value
}

const mouthopen = () => {
  goose_mouth_image.value = goose_mouth_image_open.value
}

// เปลี่ยนภาพไปเซ็ตแบบที่1
const changeImagegoose = () => {
  score_from_browser = score_count.value
  goose_mouth_image_close.value = "./gooseImages/goose_mouth_close1.png";
  goose_mouth_image_open.value = "./gooseImages/goose_mouth_open1.png";
  goose_mouth_image.value = goose_mouth_image_close.value
  score_count.value = score_from_browser1
  changeBackgroundBadGoose()
}

// เปลี่ยนภาพกลับไปเซ็ตแบบปกติ
const changeImagegoose1 = () => {
  score_from_browser1 = score_count.value
  goose_mouth_image_close.value = "./gooseImages/goose_mouth_close.png";
  goose_mouth_image_open.value = "./gooseImages/goose_mouth_open.png";
  goose_mouth_image.value = goose_mouth_image_close.value
  score_count.value = score_from_browser
  changeBackgroundGoose()
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
const old_score = ref(0)

function setScore(score) {
  old_score.value = score_count.value
  score_count.value = score
  if (goose_mouth_image_close.value === "./gooseImages/goose_mouth_close.png" ||
    goose_mouth_image_open.value === "./gooseImages/goose_mouth_open.png") {
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
  if (goose_mouth_image_close.value === "./gooseImages/goose_mouth_close.png"||
  goose_mouth_image_open.value ==="./gooseImages/goose_mouth_open.png") {
    setScore(score_count.value + (1 * PresentMultiple.value))
  } else {
    setScore(score_count.value - (1 * PresentMultiple.value))
  }
  
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
function MinigameMath() {
  if (score_count.value % 13 === mathran || score_count.value % 17 === mathran) {
    displayshow = "display:none"
    display2show = "display:block"
    math1 = 0
    math2 = 0
    mathran = Math.ceil(Math.random() * 10);
    mathran2 = Math.ceil(Math.random() * 4)
    do {
      math1 = Math.ceil(Math.random() * 10000);
      math2 = Math.ceil(Math.random() * 10000);
    } while (math1 < math2)
    if (mathran2 === 0) {
      MathOperation = "+"
      mathsum = math1 + math2
    }
    if (mathran2 === 1) {
      MathOperation = "-"
      mathsum = math1 - math2
    }
    if (mathran2 === 2) {
      MathOperation = "x"
      mathsum = math1 * math2
    }
    if (mathran2 === 3) {
      MathOperation = "÷"
      mathsum = (math1 / math2).toFixed(2)
    }

    setTimeout(AnswerMinigame, 20000)
  }
}
function AnswerMinigame() {
  if (InputNumber.value == mathsum) {
    score_count.value = score_count.value + (Math.floor(score_count.value / 10))
    clearTimeout(setTimeout(AnswerMinigame, 10000))
  }
  if (InputNumber.value != mathsum) {
    score_count.value = Math.floor(score_count.value / 2)
    clearTimeout(setTimeout(AnswerMinigame, 10000))
  }
  math1 = 0
  math2 = 0
  displayshow = "display:block"
  display2show = "display:none"
}


//Guessing game from pictures game
const guessingQuestion = ref([
  { image1: './guessinggame/เย็น.png', image2: './guessinggame/สี่ตา.jpg', anwser: 'เย็นตาโฟ' },
  { image1: './guessinggame/ลิ้น.jpg', image2: './guessinggame/ข้าวจี่่.jpg', anwser: 'ลิ้นจี่' },
  { image1: './guessinggame/โล.jpg', image2: './guessinggame/ตี.jpg', anwser: 'โรตี' },
  { image1: './guessinggame/กุ้ง.jpg', image2: './guessinggame/เต้น.jpg', anwser: 'กุ้งเต้น' }
])
let randomQuestion = ref(randomGuesingQ())
let YourAnwser = ref('')
let message = ref('')
let messageClass = ref('')

function checkGuessingAns() {
  if (YourAnwser.value.trim() === randomQuestion.value.anwser) {
    message.value = 'Correct!!!!'
    messageClass.value = 'text-green-600 mt-4 font-semibold'
  } else {
    message.value = 'Wrong!!!! Let’s try again'
    messageClass.value = 'text-red-600 mt-4 font-semibold'
  }
}
function newGuessingQuestion() {
  randomQuestion.value = randomGuesingQ()
  YourAnwser.value = ''
  message.value = ''
  messageClass.value = ''
}
function randomGuesingQ() {
  return guessingQuestion.value[Math.floor(Math.random() * guessingQuestion.value.length)]
}


//Change Background 
const bgWindowXP = './backgroundImages/WindowsXp.jpg'
const bgFlame = './backgroundImages/Fireflam.jpg'
const bgBrain = './backgroundImages/Brain.jpg'
const bgMountain = './backgroundImages/mountain.jpg'
const bgSea = './backgroundImages/sea.jpg'
const bgVolcano = './backgroundImages/volcano.jpg'
const bgSpace = './backgroundImages/space.jpg'

const background = ref(bgWindowXP) //Original Background

function changeBackground() {
  if (goose_mouth_image_close.value === "./gooseImages/goose_mouth_close.png"||
  goose_mouth_image_open.value ==="./gooseImages/goose_mouth_open.png") {
    changeBackgroundGoose()
  }else{
    changeBackgroundBadGoose()
  }
}



function changeBackgroundGoose() {
  if (score_count.value <= 500) {
    background.value = bgWindowXP
  } else if (score_count.value > 500 && score_count.value <= 1500) {
    background.value = bgMountain
  } else if (score_count.value > 1500 && score_count.value <= 3000) {
    background.value = bgSea
  } else if (score_count.value > 3000 && score_count.value <= 4500) {
    background.value = bgFlame
  } else if (score_count.value > 4500 && score_count.value <= 7000) {
    background.value = bgVolcano
  } else if (score_count.value > 7000 && score_count.value <= 10000) {
    background.value = bgSpace
  } else {
    background.value = bgBrain
  }
}

function changeBackgroundBadGoose() {
  if (score_count.value >= -500) {
    background.value = bgWindowXP
  } else if (score_count.value < -500 && score_count.value >= -1500) {
    background.value = bgMountain
  } else if (score_count.value < -1500 && score_count.value >= -3000) {
    background.value = bgSea
  } else if (score_count.value < -3000 && score_count.value >= -4500) {
    background.value = bgFlame
  } else if (score_count.value < -4500 && score_count.value >= -7000) {
    background.value = bgVolcano
  } else if (score_count.value < -7000 && score_count.value >= -10000) {
    background.value = bgSpace
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

//Minigame3 (Poom) Part -----------------------------------------------------------------------------------------------------------------------------------------------------

const minigame3On = ref(false)
const unitNameBefore = ref('')
const unitNameAfter = ref('')
const beforeConvert = ref(0)
const userAnswer = ref(0)
const scoreGiveForWin = ref(0)

const openMiniGame3 = () => {
  isOpen.value = false
  minigame3On.value = true
  randomQuestionOfGame3();
  setTimeout(closeMiniGame3, 15000)
}

const closeMiniGame3 = () => {
  minigame3On.value = false
}

function getRandomIntInclusive(min, max) {
  const minCeiled = Math.ceil(min);
  const maxFloored = Math.floor(max);
  return Math.floor(Math.random() * (maxFloored - minCeiled + 1) + minCeiled); // The maximum is inclusive and the minimum is inclusive
}

function getRandomIntMultipleOf60(min, max) {
  // ปัด min ให้เป็นค่าที่หารด้วย 60 ลงตัว (เช่น 61 จะปัดไปที่ 60)
  const minMultipleOf60 = Math.floor(min / 60) * 60;

  // ปัด max ให้เป็นค่าที่หารด้วย 60 ลงตัว (เช่น 599 จะปัดไปที่ 600)
  const maxMultipleOf60 = Math.floor(max / 60) * 60;

  // สุ่มเลขระหว่าง min และ max ที่หารด้วย 60 ลงตัว
  const randomMultipleOf60 = Math.floor(Math.random() * ((maxMultipleOf60 - minMultipleOf60) / 60 + 1)) * 60 + minMultipleOf60;
  return randomMultipleOf60;
}

function randomQuestionOfGame3() {
  unitNameBefore.value = (getRandomIntInclusive(1, 2) === 1) ? 'Hour' : 'Minute';
  unitNameAfter.value = (unitNameBefore.value === 'Minute') ? 'Hour' : 'Minute';
  if (unitNameBefore.value === 'Hour') {
    beforeConvert.value = getRandomIntInclusive(1, 24);
  }
  else {
    beforeConvert.value = getRandomIntMultipleOf60(60, 1440); //เอาเฉพาะนาทีที่หาง่าย ๆ และไม่เกิน 24 ชั่วโมง
  }
}

watch(
  userAnswer, (newValue, oldValue) => {
    if (unitNameBefore.value === 'Hour') {
      if (newValue === beforeConvert.value * 60) {
        scoreGiveForWin.value = getRandomIntInclusive(10, 100)
        setScore(score_count.value + scoreGiveForWin.value)
        userAnswer.value = ""
        congratPlayerWin()
      }
    }
    else {
      if (newValue === beforeConvert.value / 60) {
        scoreGiveForWin.value = getRandomIntInclusive(10, 100)
        setScore(score_count.value + scoreGiveForWin.value)
        userAnswer.value = ""
        congratPlayerWin()
      }
    }
  }
)

//Minigame Winner Page Part -----------------------------------------------------------------------------------------------------------------------------------------------------

const minigameWin = ref(false)

const congratPlayerWin = () => {
  closeMiniGame3()
  minigameWin.value = true
  setTimeout(closeCongratPlayerWin, 2000)
}

const closeCongratPlayerWin = () => {
  minigameWin.value = false
}

</script>

<template>
  <div>
    <!-- Main Page of PushGoose  -->
    <div v-bind:style="displayshow" v-if="minigame3On !== true && minigameWin !== true">
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
              @click="increaseCount(); RandomMultiple(); changeBackground(); MinigameMath()" @mousedown="mouthopen"
              @mouseup="mouthclose" />

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
        hover:scale-110 cursor-pointer" v-bind:src="currentVolumeIcon"></button>
        <select @change="changeImage"  class="px-6 py-3 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600 active:scale-95 shadow-lg transition-all 
              duration-300 cursor-pointer">
                <option value="Goose">Goose</option>
                <option value="BadGoose">Bad</option>
                </select>
            </div>
          </div>
          <!-- mini game button -->
          <div class="absolute top-45 right-15 size-14">
            <button class="px-6 py-3 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600 transition delay-150 duration-300 ease-in-out hover:-translate-y-1 
        hover:scale-110 hover:bg-indigo-500 cursor-pointer" @click="openMiniGameModal">Mini Game</button>
          </div>
          <!-- modal -->
          <div v-if="isOpen"
            class="fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 border-black border rounded-lg z-10 bg-purple-300 w-350 h-200">
            <!-- modal header -->
            <div v-if="isOpen"
              class="p-4 px-8 flex justify-between items-center border-black border-b text-xl font-bold">
              <div class="text-xl font-bold">Mini Game</div>
              <button @click="closeMiniGameModal"
                class="fond-bold text-3xl cursor-pointer transition-transform duration-0.2">&times;</button>
            </div>
            <!-- modal body -->
            <div class="p-4 px-4 flex justify-center">
              <!-- Math Quiz Button -->
              <div class="grid grid-flow-col mt-15 justify-center gap-12">
                <button class="cursor-pointer"><img src="./assets/testModalPic/herta1.jpg"
                    class="w-100 h-auto border-2 border-transparent rounded-lg transition-transform duration-300 hover:scale-110 hover:border-red-500 border-5">
                </button>
                <button class="cursor-pointer"><img src="./assets/testModalPic/herta2.jpg"
                    class="w-100 h-auto border-2 border-transparent rounded-lg transition-transform duration-300 hover:scale-110 hover:border-red-500 border-5">
                </button>
                <button @click="openMiniGame3" class="cursor-pointer"><img src="./assets/testModalPic/herta3.jpg"
                    class="w-100 h-auto border-2 border-transparent rounded-lg transition-transform duration-300 hover:scale-110 hover:border-red-500 border-5">
                </button>
              </div>
            </div>
          </div>
          <div class="fixed top-0 left-0 bottom-0 right-0 pointer-events-none"></div>
        </div>
      </div>
    </div>

    <!-- Minigame of PushGoose  -->
    <!-- MathGame UI -->
    <div v-bind:style="display2show">
      <div class="flex items-center justify-center min-h-screen bg-gray-100 m-0">
        <div class="bg-white p-8 rounded shadow-md w-full max-w-md">
          <h1 class="text-2xl font-bold mb-4">Number Quiz Game</h1>
          <p class="mb-4">Enter the result of the operation:</p>
          <div id="question" class="text-lg font-semibold mb-4">{{ math1 }} {{ MathOperation }} {{ math2 }} = {{
            InputNumber
          }}</div>
          <input type="number" id="answer" class="border rounded w-full py-2 px-3 mb-4" placeholder="Your answer"
            v-model.numbers="InputNumber">
          <p class="mb-4 justify-center">- Answer Within 20 Second</p>
          <p class="mb-2 justify-center">- If Operation is divide. Answer With Two Decimal Place </p>
        </div>

        <!-- GuessingGame UI -->
        <div class="flex flex-col items-center justify-center min-h-screen bg-gray-100 m-0 ">
          <div class="bg-white p-8 rounded shadow-md w-full max-w-md">
            <h1 class="text-2xl font-bold mb-4">Guessing from pictures</h1>
            <div class="flex justify-center gap-2 mb-4">
              <img :src="randomQuestion.image1" alt="question img1" class="w-40 h-40 rounded shadow">
              <img :src="randomQuestion.image2" alt="question img2" class="w-40 h-40 rounded shadow">
            </div>
            <p class="mb-4">Enter your anwser : {{ InputText }}</p>
            <input v-model="YourAnwser" type="text" class="border rounded w-full py-2 px-3 mb-4"
              placeholder="Your answer">
            <div class="flex gap-2">
              <button class="px-4 py-2 text-white font-semibold rounded-2xl bg-green-500 hover:bg-green-600"
                @click="checkGuessingAns">Submit</button>
              <button class="px-4 py-2 text-white font-semibold rounded-2xl bg-blue-500 hover:bg-blue-600"
                @click="newGuessingQuestion">New Question</button>
            </div>

            <p :class="messageClass">{{ message }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Minigame3 (Poom) of PushGoose  -->
    <div v-if="minigame3On === true"
      class="flex flex-col items-center justify-center min-h-screen bg-gradient-to-r from-blue-400 to-green-400 p-6 text-white">
      <h1 class="text-4xl font-bold mb-2 drop-shadow-lg">Unit Converter Game</h1>
      <h2 class="text-lg sm:text-2xl text-center mb-6 font-light">กรุณาตอบให้ถูกต้องภายในเวลา 15 วินาที</h2>

      <div class="bg-white p-6 rounded-lg shadow-lg max-w-lg w-full">
        <div class="flex flex-col sm:flex-row items-center space-y-4 sm:space-y-0 sm:space-x-4 mb-4">
          <div class="border p-4 rounded w-full flex flex-col bg-gray-100 text-gray-700">
            <input disabled type="number"
              class="border-none w-full text-center text-2xl py-2 bg-transparent focus:outline-none"
              :value="beforeConvert" />
            <select disabled class="border-t p-2 w-full bg-gray-300 text-gray-700 text-sm rounded">
              <option selected>{{ unitNameBefore }}</option>
            </select>
          </div>
          <span class="text-3xl font-bold text-gray-800">=</span>
          <div class="border p-4 rounded w-full flex flex-col bg-gray-100 text-gray-700">
            <input type="number" class="border-none w-full text-center text-2xl py-2 bg-transparent focus:outline-none"
              v-model="userAnswer" placeholder="พิมคำตอบที่นี่" />
            <select disabled class="border-t p-2 w-full bg-gray-300 text-gray-700 text-sm rounded">
              <option selected>{{ unitNameAfter }}</option>
            </select>
          </div>
        </div>
        <button
          class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded transition duration-300">❗หากตอบถูกระบบจะปิดหน้าต่างนี้อัตโนมัติ</button>
      </div>
    </div>

    <!-- Winner Page of PushGoose  -->
    <div v-if="minigameWin"
      class="flex flex-col items-center justify-center min-h-screen bg-gradient-to-r from-blue-600 to-green-600 p-6 text-white text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-2 drop-shadow-lg">Answer Correct !</h1>
      <p class="text-lg md:text-xl">You earned <span class="font-semibold">{{ score_count - old_score }}</span> points!
      </p>
    </div>

  </div>
</template>

<style scoped></style>