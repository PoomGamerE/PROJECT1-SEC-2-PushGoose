<script setup>
import { ref } from 'vue'

//goose img zone
const goose_mouth_image = ref('./gooseImages/goose_mouth_close.png')

//score zone
const score_count = ref(0) //reactive variable

//for Multiple
let nclick = 0
let PresentMultiple = ref(1)
let MColor = "color:black"

const increaseCount = () => {
  goose_mouth_image.value = './gooseImages/goose_mouth_open.png'
  score_count.value = score_count.value+(1*PresentMultiple.value)
  //goose_mouth_image.value = './gooseImages/goose_mouth_close.png'
}

//Random Multiple 
function RandomMultiple(){
    nclick++ //นับว่าคลิกไปกี่ครั้งแล้วหลังจากไม่ได้ตัวคูณ
    const ran =Math.ceil(Math.random()*30);
    if(ran-nclick>0){ //ยิ่งกดไม่ติดเยอะ โอกาศยิ่งเพิ่มขึ้นการันตีอยู่ที่30ครั้ง
        PresentMultiple.value = 1 //รีเซ็ตตัวคูณ
        MColor = "color:black"
        return PresentMultiple.value
    } else if(ran-nclick<=0){
        PresentMultiple.value = Math.ceil(Math.random()*10) //สุ่มตัวคูณ1-10
        nclick = 0 //รีเซ็ตค่าคลิก
        if(PresentMultiple.value>3){
          MColor = "color:green"
          if(PresentMultiple.value>6){
            MColor = "color:blue"
            if(PresentMultiple.value>9){
              MColor = "color:red"
            }
          }
        }
        return PresentMultiple.value
    }
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
        <img class="w-full h-full" v-bind:src="goose_mouth_image" v-on:click="increaseCount();RandomMultiple()"/>
      
    </div>
    <!-- Multiple -->
    <div>
      <h2 class= "text-10 italic font-bold  text-center" v-bind:style="MColor" v-text="`X`+PresentMultiple"></h2>   
    </div>
  </div>
</div>
  
</template>

<style scoped></style>