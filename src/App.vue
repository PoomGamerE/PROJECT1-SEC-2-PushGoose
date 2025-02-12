<script setup>
import { ref, watch } from 'vue'

//Sound
const volumeOn = './volumeOn_Off/volume_on.png' //volume-on.png
const volumeOff = './volumeOn_Off/volume_off.png' //volume-off.png
const currentVolumeIcon = ref(volumeOn) //reactive variable

const whenClicked = () => {
  currentVolumeIcon.value =
    currentVolumeIcon.value === volumeOn ? volumeOff : volumeOn;
  // if the currentVolumeIcon.value = volumeOn is TRUE the value is going to be volumeOff
  // if not it's volumeOn
}

//goose img zone
const goose_mouth_image_open = ref('./gooseImages/goose_mouth_open.png');
const goose_mouth_image_close = ref('./gooseImages/goose_mouth_close.png');
const goose_mouth_image = ref(goose_mouth_image_close.value)
const Goose =ref('Goose')

//Change img zone
const changeImage = (event) => {
  if (event.target.value === "BadGoose") {
    changeImagegooseBadGoose()
  } else if (event.target.value === "Goose")
    changeImagegoose()
}

const mouthclose = () => {
  goose_mouth_image.value = goose_mouth_image_close.value
}

const mouthopen = () => {
  goose_mouth_image.value = goose_mouth_image_open.value
}

// เปลี่ยนภาพไปเซ็ตเป็นแบบBadGoose
const changeImagegooseBadGoose = () => {
  score_from_browser_BadGoose = Number(localStorage.getItem('scoreBadGoose')) || 0
  score_count.value = score_from_browser_BadGoose
  goose_mouth_image_close.value = "./gooseImages/goose_mouth_close_BadGoose.png";
  goose_mouth_image_open.value = "./gooseImages/goose_mouth_open_BadGoose.png";
  goose_mouth_image.value = goose_mouth_image_close.value
  Goose.value="BadGoose"
  changeBackgroundBadGoose()
}

// เปลี่ยนภาพกลับไปเซ็ตแบบปกติ
const changeImagegoose = () => {
  score_from_browser_Goose = Number(localStorage.getItem('scoreGoose')) || 0
  score_count.value = score_from_browser_Goose
  goose_mouth_image_close.value = "./gooseImages/goose_mouth_close.png";
  goose_mouth_image_open.value = "./gooseImages/goose_mouth_open.png";
  goose_mouth_image.value = goose_mouth_image_close.value
  changeBackgroundGoose()
  Goose.value="Goose"
}
//score zone
// คะแนนgooseแบบปกติ
let score_from_browser_Goose = localStorage.getItem('scoreGoose')
if (score_from_browser_Goose === null) {
  localStorage.setItem('scoreGoose', 0)
  score_from_browser_Goose = 0
}
else {
  score_from_browser_Goose = Number.parseInt(score_from_browser_Goose)
}
// คะแนนgooseแบบที่เป็นBadGoose
let score_from_browser_BadGoose = localStorage.getItem('scoreBadGoose')
if (score_from_browser_BadGoose === null) {
  localStorage.setItem('scoreBadGoose', 0)
  score_from_browser_BadGoose = 0
}
else {
  score_from_browser_BadGoose = Number.parseInt(score_from_browser_BadGoose)
}
const score_count = ref(score_from_browser_Goose) //reactive variable
const old_score = ref(0)

function setScore(score) {
  old_score.value = score_count.value
  score_count.value = score
  if (Goose.value==="Goose") {
    localStorage.setItem('scoreGoose', score)
  } else {
    localStorage.setItem('scoreBadGoose', score)
  }
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
  if (Goose.value==="Goose") {
    changeBackgroundGoose()
  } else {
    changeBackgroundBadGoose()
  }
}



function changeBackgroundGoose() {
  if (score_count.value > 10000) {
    background.value = bgBrain
  } else if (score_count.value > 7000 ) {
    background.value =  bgSpace
  } else if (score_count.value > 4500 ) {
    background.value = bgVolcano
  } else if (score_count.value > 3000 ) {
    background.value = bgFlame
  } else if (score_count.value > 1500 ) {
    background.value = bgSea
  } else if (score_count.value > 500) {
    background.value = bgMountain
  } else {
    background.value = bgWindowXP
  }
}
 
function changeBackgroundBadGoose() {
  if (score_count.value < -10000) {
    background.value = bgBrain
  } else if (score_count.value < -7000 ) {
    background.value =  bgSpace
  } else if (score_count.value < -4500 ) {
    background.value = bgVolcano
  } else if (score_count.value < -3000 ) {
    background.value = bgFlame
  } else if (score_count.value < -1500 ) {
    background.value = bgSea
  } else if (score_count.value < -500) {
    background.value = bgMountain
  } else {
    background.value = bgWindowXP
  }
}

//for Multiple
let nclick = 0
let PresentMultiple = ref(1)
let MColor = "color:black"

const increaseCount = () => {
  if (Goose.value==="Goose") {
    setScore(score_count.value + (1 * PresentMultiple.value))
  } else {
    setScore(score_count.value - (1 * PresentMultiple.value))
  }

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
let displayshow = true
let display2show = false
let math1 = 0
let math2 = 0
let mathsum = 0
let mathran2 = Math.ceil(Math.random() * 3);
let MathOperation = "+"
let InputNumber = ref(0)
let Timeouts = []

function MinigameMath() {
  InputNumber.value = ""
  displayshow = false
  display2show = true
  isMiniGameModalOpen.value = false
  math1 = 0
  math2 = 0
  mathran2 = Math.ceil(Math.random() * 3)
  math1 = Math.ceil(Math.random() * 10000) + 3000;
  math2 = Math.ceil(Math.random() * 2000) + 1000;
  if (mathran2 === 0) {
    MathOperation = "+"
    mathsum = math1 + math2
  }
  if (mathran2 === 1) {
    MathOperation = "-"
    mathsum = math1 - math2
  }
  if (mathran2 === 2) {
    MathOperation = "×"
    mathsum = math1 * math2
  }
  if (Timeouts.length === 0 && mathsum !== 0 && mathsum !== undefined && mathsum !== null) {
    let Timeout = setTimeout(LosingMinigame, 30000)
    Timeouts.push(Timeout)
  } else { // ดักเผื่อ timeout ซ้อนกัน และกันไม่มีตัวเลขเมื่อกดเล่น minigame
    InputNumber.value = 0
    math1 = 0
    math2 = 0
    displayshow = true
    display2show = false
    Timeouts.forEach(id => clearTimeout(id));
    Timeouts = []
    MinigameMath()
  }
}


watch([InputNumber], () => {
  if (InputNumber.value == mathsum && InputNumber.value !== 0) {
    AddGameScore((Math.floor(score_count.value / 10)))
    Timeouts.forEach(id => clearTimeout(id)) // ใช้อันนี้เพราะหาวิธีแก้Timeoutไม่ได้เลยลบให้หมดเลย ใช้clearTimeoutปกติไม่ได้(ไม่รู้ทำไมเหมือนกัน)
    Timeouts = []
    MathcongratPlayerWin()
    InputNumber.value = 0
    math1 = 0
    math2 = 0
    mathsum = 0
    displayshow = true
    display2show = false
  }
})


function LosingMinigame() {
  if (mathsum !== 0) {
    reduceGameScore((Math.floor(score_count.value / 2)))
    setScore(newscore)
    Timeouts.forEach(id => clearTimeout(id))
    Timeouts = []
    MathcongratPlayerWin()
    math1 = 0
    math2 = 0
    mathsum = 0
    InputNumber.value = 0
    displayshow = true
    display2show = false
  }
}


//Guessing game from pictures game
const guessingQuestion = [
  { image1: './guessinggame/เย็น.png', image2: './guessinggame/สี่ตา.jpg', answer: 'เย็นตาโฟ' },
  { image1: './guessinggame/ลิ้น.jpg', image2: './guessinggame/ข้าวจี่่.jpg', answer: 'ลิ้นจี่' },
  { image1: './guessinggame/โล.jpg', image2: './guessinggame/ตี.jpg', answer: 'โรตี' },
  { image1: './guessinggame/กุ้ง.jpg', image2: './guessinggame/เต้น.jpg', answer: 'กุ้งเต้น' }
]
let randomQuestion = ref(randomGuesingQ())
let YourAnswer = ref('')
let message = ref('')
let messageClass = ref('')
let displayMiniG2 = false

function checkGuessingAns() {
  if (YourAnswer.value.trim() === randomQuestion.value.answer) {
    AddGameScore((Math.floor(score_count.value / 10)))
    message.value = 'Correct!!!!'
    messageClass.value = 'text-green-600 mt-8 text-center font-bold'
    setTimeout(congratMiniG2PlayerWin, 500)

  } else {
    reduceGameScore((Math.floor(score_count.value / 5)))
    message.value = 'Wrong!!!! Let’s try again'
    messageClass.value = 'text-red-600 mt-8 text-center font-bold'
    setTimeout(closeMiniG2Lose, 2000)
  }
}

function newGuessingQuestion() {
  isMiniGameModalOpen.value = false
  displayMiniG2 = true
  displayshow = false
  randomQuestion.value = randomGuesingQ()
  YourAnswer.value = ''
  message.value = ''
  messageClass.value = ''
}
function randomGuesingQ() {
  return guessingQuestion[Math.floor(Math.random() * guessingQuestion.length)]
}



// reactive variable
const isMiniGameModalOpen = ref(false)
const currentTutorialPage = ref(0) // Tracks the current tutorial page
const totalTutorialPages = 3; // Total number of tutorial pages

//open & close MiniGameModal and TutorialModal
const openMiniGameModal = () => {
  isMiniGameModalOpen.value = true
}
const closeMiniGameModal = () => {
  isMiniGameModalOpen.value = false
}

const openTutorialModal = () => {
  currentTutorialPage.value = 1
}

const closeTutorialModal = () => {
  currentTutorialPage.value = 0
}

const nextTutorial = () => {
  if (currentTutorialPage.value < totalTutorialPages) {
    currentTutorialPage.value++
  }
}

const prevTutorial = () => {
  if (currentTutorialPage.value > 1) {
    currentTutorialPage.value--
  }
}
//Minigame3 (Poom) Part -----------------------------------------------------------------------------------------------------------------------------------------------------

const minigame3On = ref(false)
const unitNameBefore = ref('')
const unitNameAfter = ref('')
const beforeConvert = ref(0)
const userAnswer = ref(0)
const scoreGiveForWin = ref(0)

const openMiniGame3 = () => {
  isMiniGameModalOpen.value = false
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
  userAnswer.value = ""
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
        AddGameScore(scoreGiveForWin.value)
        userAnswer.value = ""
        clearTimeout(setTimeout(closeMiniGame3, 15000))
        congratPlayerWin()
      }
    }
    else {
      if (newValue === beforeConvert.value / 60) {
        scoreGiveForWin.value = getRandomIntInclusive(10, 100)
        AddGameScore(scoreGiveForWin.value)
        userAnswer.value = ""
        clearTimeout(setTimeout(closeMiniGame3, 15000))
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
const congratMiniG2PlayerWin = () => {
  displayMiniG2 = false
  minigameWin.value = true
  setTimeout(closeCongratPlayerWin, 2000)
}
const MathcongratPlayerWin = () => {
  minigameWin.value = true
  setTimeout(closeCongratPlayerWin, 2000)
}
const closeMiniG2Lose = () => {
  displayshow = true
  displayMiniG2 = false
}
const closeCongratPlayerWin = () => {
  displayshow = true
  minigameWin.value = false
}

//เพิ่มคะแนนเมือจบเกม--------
const AddGameScore = (score) => {
  if (Goose.value==="Goose") {
    setScore(score_count.value + score)
    changeBackground()
  } else {
    setScore(score_count.value - score)
    changeBackground()
  }

}

//ลดคะแนน
const reduceGameScore = (score) => {
  if (Goose.value==="Goose") {
    setScore(score_count.value - score)
    changeBackground()
  } else {
    setScore(score_count.value + score)
    changeBackground()
  }
}

//Minigame4 (Hafiz,Gain) Part --------------------------
const options = ref([
  { name: "rock", label: "หิน", emoji: "✊" },
  { name: "paper", label: "กระดาษ", emoji: "✋" },
  { name: "scissors", label: "กรรไกร", emoji: "✌️" }
]);



changeBackground()
</script>

<template>
  <div>
    <!-- Main Page of PushGoose -->
    <div v-if="minigame3On !== true && minigameWin !== true && displayshow" class="overflow-hidden h-screen">
      <div v-bind:style="{ backgroundImage: `url(${background})` }"
        class="bg-no-repeat bg-cover flex flex-col items-center justify-center h-screen p-4">
        <!-- Title with Text Outline (ปรับสีให้สวยงามและรองรับพื้นหลัง) -->
        <h1 class="text-7xl font-bold text-transparent bg-clip-text bg-gradient-to-r text-white
             text-center px-4 py-2 rounded-md"
          style="text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.9), 6px 6px 12px rgba(0, 0, 0, 0.8);">
          PushGoose
        </h1>

        <!-- Score Count with Text Outline -->
        <h1 class="text-5xl font-bold text-yellow-700 bg-clip-text bg-gradient-to-r from-yellow-600 to-yellow-800
             text-center px-4 py-2 rounded-md mt-1" style="text-shadow: 
           0px 0px 4px rgba(255, 255, 255, 0.9), 
           0px 0px 8px rgba(255, 255, 255, 0.6);">
          {{ score_count }}
        </h1>

        <!-- Goose Image (No Border, Adjusted Size) -->
        <div class="w-3/4 sm:w-2/3 md:w-1/2 lg:w-1/3 xl:w-1/4 mt-6">
          <img class="w-full h-full cursor-pointer rounded-none" v-bind:src="goose_mouth_image"
            @click="increaseCount(); RandomMultiple(); changeBackground();" @mousedown="mouthopen"
            @mouseup="mouthclose" />
        </div>

        <!-- Multiple Display with Text Outline -->
        <h2 class="text-3xl font-bold italic text-white text-center px-6 py-2 mt-4 rounded-md" v-bind:style="MColor"
          v-text="`X` + PresentMultiple"
          style="text-shadow: 0px 0px 6px rgba(255, 255, 255, 1), 0px 0px 12px rgba(255, 255, 255, 0.8), 4px 4px 8px rgba(0, 0, 0, 0.7);">
        </h2>

        <!-- Volume & Image Selector -->
        <div class="absolute top-5 right-5 flex gap-4">
          <!-- Volume Toggle -->
          <button v-on:click="whenClicked"
            class="w-14 h-14 flex items-center justify-center bg-gray-200 rounded-full hover:bg-gray-300 transition cursor-pointer">
            <img class="w-10 h-10" v-bind:src="currentVolumeIcon" />
          </button>

          <!-- Change Goose Type -->
          <select @change="changeImage"
            class="px-4 py-2 text-white font-semibold rounded-lg bg-green-500 hover:bg-green-600 transition cursor-pointer">
            <option value="Goose">Goose</option>
            <option value="BadGoose">Bad</option>
          </select>
        </div>

        <!-- Mini Game Button (อยู่ขวาบนเหมือนเดิม) -->
        <div class="absolute top-21 right-5">
          <button class="px-6 py-3 text-white font-semibold rounded-xl bg-indigo-500 hover:bg-indigo-600 
            transition-all duration-300 hover:-translate-y-1 hover:scale-110 cursor-pointer"
            @click="openMiniGameModal">
            Mini Game
          </button>
        </div>

        <!-- Mini Game Modal -->
        <div v-if="isMiniGameModalOpen"
          class="fixed inset-0 flex items-center justify-center bg-black/50 backdrop-blur-sm z-50"
          @click.self="closeMiniGameModal">
          <div class="bg-white max-w-md sm:max-w-lg w-full rounded-lg overflow-hidden">
            <!-- Mini Game Modal Header -->
            <div class="p-4 flex justify-between items-center bg-gray-100 border-b">
              <h2 class="text-lg font-semibold">Mini Game</h2>
              <button @click="closeMiniGameModal"
                class="text-2xl font-bold text-gray-600 hover:text-red-500 transition cursor-pointer">
                &times;
              </button>
            </div>
            <!-- Mini Game Modal Body -->
            <div class="p-6 flex flex-col items-center">
              <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                <!-- Number Quiz Game -->
                <div class="flex flex-col items-center">
                  <button @click="MinigameMath">
                    <img src="./assets/miniGamePic/numberQuiz.jpg"
                      class="w-24 h-24 object-cover rounded-lg shadow-md hover:shadow-xl transition-transform duration-300 hover:scale-105 cursor-pointer">
                  </button>
                  <p class="text-center text-sm text-gray-700 font-medium mt-2">Number Quiz</p>
                  <p class="text-center text-xs text-gray-500">ตอบตัวเลขให้ถูกต้อง</p>
                </div>

                <!-- Guessing from Pictures Game -->
                <div class="flex flex-col items-center">
                  <button @click="newGuessingQuestion">
                    <img src="./assets/miniGamePic/guessingFromPic.jpg"
                      class="w-24 h-24 object-cover rounded-lg shadow-md hover:shadow-xl transition-transform duration-300 hover:scale-105 cursor-pointer">
                  </button>
                  <p class="text-center text-sm text-gray-700 font-medium mt-2">Guessing from Pic</p>
                  <p class="text-center text-xs text-gray-500">ทายคำจากภาพ</p>
                </div>

                <!-- Unit Converter Game -->
                <div class="flex flex-col items-center">
                  <button @click="openMiniGame3">
                    <img src="./assets/miniGamePic/unitConverterGame.jpg"
                      class="w-24 h-24 object-cover rounded-lg shadow-md hover:shadow-xl transition-transform duration-300 hover:scale-105 cursor-pointer">
                  </button>
                  <p class="text-center text-sm text-gray-700 font-medium mt-2">Unit Converter</p>
                  <p class="text-center text-xs text-gray-500">แปลงหน่วยให้ถูกต้อง</p>
                </div>

                <!-- New Mini Game -->
                <div class="flex flex-col items-center">
                  <button @click="openMiniGame4">
                    <img src="./assets/miniGamePic/newMiniGame.jpg"
                      class="w-24 h-24 object-cover rounded-lg shadow-md hover:shadow-xl transition-transform duration-300 hover:scale-105 cursor-pointer">
                  </button>
                  <p class="text-center text-sm text-gray-700 font-medium mt-2">New Mini Game</p>
                  <p class="text-center text-xs text-gray-500">รายละเอียดเกมใหม่</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="fixed top-0 left-0 bottom-0 right-0 pointer-events-none"></div>
        <!-- Tutorial Button -->
        <div class="absolute top-35 right-5">
          <button class="px-6 py-3 text-white font-semibold rounded-xl bg-indigo-500 hover:bg-indigo-600 
            transition-all duration-300 hover:-translate-y-1 hover:scale-110 cursor-pointer"
            @click="openTutorialModal">
            How To Play
          </button>
        </div>

        <!-- Backdrop (blurred background) -->
        <div v-if="currentTutorialPage > 0" class="fixed inset-0 backdrop-blur-sm pointer-events-auto z-10"
          @click="closeTutorialModal">
        </div>

        <!-- Tutorial Modal -->
        <div v-if="currentTutorialPage > 0">
          <div class="fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2
    border border-black rounded-lg z-10 bg-orange-100 max-w-sm sm:max-w-lg w-full p-4 sm:p-6" @click.stop>

            <!-- Page 1 -->
            <div v-if="currentTutorialPage === 1">
              <div class="flex justify-between items-center border-b border-black pb-2 text-lg sm:text-xl font-bold">
                <div>#1 What is the purpose of this game?</div>
                <button class="text-2xl font-bold text-gray-600 hover:text-red-500 transition cursor-pointer"
                  @click="closeTutorialModal">&times;</button>
              </div>
              <div class="mt-3">
                <p>If you click on the goose, you will get points, and sometimes the points will be randomly multiplied.
                </p>
              </div>
              <div class="flex justify-end mt-4">
                <button @click="nextTutorial" class="cursor-pointer rounded-md bg-green-500 hover:bg-green-600 
          transition-all duration-300 hover:-translate-y-1 hover:scale-110 text-white px-4 py-2 w-full sm:w-auto">
                  Next
                </button>
              </div>
            </div>

            <!-- Page 2 -->
            <div v-if="currentTutorialPage === 2">
              <div class="flex justify-between items-center border-b border-black pb-2 text-lg sm:text-xl font-bold">
                <div>#2 Change Goose</div>
                <button class="text-2xl font-bold text-gray-600 hover:text-red-500 transition cursor-pointer"
                  @click="closeTutorialModal">&times;</button>
              </div>
              <div class="mt-3">
                <p>You can change it to a bad goose, but it will cost different points than the good goose, and the
                  points will be negative instead.</p>
              </div>
              <div class="flex justify-between mt-4">
                <button @click="prevTutorial" class="cursor-pointer rounded-md bg-red-500 hover:bg-red-600 
          transition-all duration-300 hover:-translate-y-1 hover:scale-110 text-white px-4 py-2 w-full sm:w-auto">
                  Back
                </button>
                <button @click="nextTutorial" class="cursor-pointer rounded-md bg-green-500 hover:bg-green-600 
          transition-all duration-300 hover:-translate-y-1 hover:scale-110 text-white px-4 py-2 w-full sm:w-auto">
                  Next
                </button>
              </div>
            </div>

            <!-- Page 3 -->
            <div v-if="currentTutorialPage === 3">
              <div class="flex justify-between items-center border-b border-black pb-2 text-lg sm:text-xl font-bold">
                <div>#3 What is Mini Game?</div>
                <button class="text-2xl font-bold text-gray-600 hover:text-red-500 transition cursor-pointer"
                  @click="closeTutorialModal">&times;</button>
              </div>
              <div class="mt-3">
                <p>We also have mini-games. The first game we have is a Number Quiz game.
                  The second game we have is a Guessing from Pic game.
                  And the last game we have is a Unit Converter game.</p>
              </div>
              <div class="flex justify-between mt-4">
                <button @click="prevTutorial" class="cursor-pointer rounded-md bg-red-500 hover:bg-red-600 
          transition-all duration-300 hover:-translate-y-1 hover:scale-110 text-white px-4 py-2 w-full sm:w-auto">
                  Back
                </button>
                <button @click="closeTutorialModal" class="cursor-pointer rounded-md bg-green-500 hover:bg-green-600 
          transition-all duration-300 hover:-translate-y-1 hover:scale-110 text-white px-4 py-2 w-full sm:w-auto">
                  Finish
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Minigame of PushGoose  -->
    <!-- MathGame UI -->
    <div v-if="display2show">
      <div class="flex items-center justify-center min-h-screen bg-gray-100 m-0">
        <div class="bg-white p-8 rounded shadow-md w-full max-w-md">
          <h1 class="text-2xl font-bold mb-4">Number Quiz Game</h1>
          <p class="mb-4">Enter the result of the operation:</p>
          <div id="question" class="text-lg font-semibold mb-4">{{ math1 }} {{ MathOperation }} {{ math2 }} = ?</div>
          <input type="number" id="answer" class="border rounded w-full py-2 px-3 mb-4" placeholder="Your answer"
            v-model.numbers="InputNumber">
          <p class="mb-4 justify-center">- Answer Within 30 Second</p>
        </div>
      </div>
    </div>
  </div>

  <!-- GuessingGame UI -->
  <div>
    <div v-if="displayMiniG2"
      class="flex items-center justify-center min-h-screen bg-gradient-to-r from-green-400 via-blue-500 to-purple-600 m-0">
      <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-xl">
        <h1 class="text-3xl font-extrabold text-center text-gray-800 mb-6">Guessing from Pictures</h1>
        <div class="flex justify-center gap-4 mb-6">
          <img :src="randomQuestion.image1" alt="question img1" class="w-48 h-48 object-cover rounded-lg shadow-lg">
          <img :src="randomQuestion.image2" alt="question img2" class="w-48 h-48 object-cover rounded-lg shadow-lg">
        </div>
        <p class="text-lg font-semibold text-gray-700 mb-4 text-center">Enter your answer in thai :</p>
        <input v-model="YourAnswer" type="text"
          class="border border-gray-300 rounded-lg w-full py-3 px-4 mb-6 focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Your answer">
        <div class="flex gap-4 justify-center">
          <button
            class="px-6 py-3 text-white font-semibold rounded-full bg-green-500 hover:bg-green-600 transition duration-300 ease-in-out transform hover:scale-105"
            @click="checkGuessingAns">Submit</button>
          <button
            class="px-6 py-3 text-white font-semibold rounded-full bg-blue-500 hover:bg-blue-600 transition duration-300 ease-in-out transform hover:scale-105"
            @click="newGuessingQuestion">New Question</button>
        </div>
        <p :class="messageClass">{{ message }}</p>
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
    
<!-- Minigame4 (Hafiz,Gain) of PushGoose  -->
<!-- <div class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-4">
    <h1 class="text-3xl font-bold mb-6">เป่ายิ้งฉุบ</h1>
    <div class="grid grid-cols-3 gap-4 mb-6 w-full max-w-md">
      <div v-for="option in options" :key="option.name" @click="play(option.name)"
        class="bg-white shadow-md rounded-lg p-6 flex flex-col items-center justify-center cursor-pointer hover:bg-gray-200 transition transform hover:scale-105">
        <span class="text-6xl">{{ option.emoji }}</span>
        <span class="mt-2 font-bold text-lg">{{ option.label }}</span>
      </div>
    </div>
    <div v-if="result" class="text-lg font-semibold mt-4">ผลลัพธ์: {{ result }}</div>
    <div class="mt-4 text-sm text-gray-700">คุณเลือก: <span class="font-bold">{{ playerChoice }}</span> | บอทเลือก: <span class="font-bold">{{ botChoice }}</span></div>
  </div> -->

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