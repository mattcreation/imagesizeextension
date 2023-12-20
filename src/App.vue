<template>
  <div class="wrapper">
    <div class="header" title="by mattcreation">
      Image Size / Ratio<br><span style="font-size:12px"><a href="https://www.facebook.com/mattcreation-app" target="_blank"> By Mattcreation</a></span></div>
    <div class="content">
      <div class="inputs imagesource">
        <div class="inputcontent">
        <label>W</label>
          <input type="number" min="1" max="999999" maxlength="6" v-model="source_width" />
        </div>
        :
        <div class="inputcontent">
          <label>H</label>
          <input type="number" min="1" max="999999" maxlength="6" v-model="source_height" />
        </div>
      </div>
      <div class="inputs imageresult">
        <div class="inputcontent">
          <input type="number" min="1" id="resultwidth" max="999999" maxlength="6" v-model="result_width"
            placeholder="Result Width" />
          <div class="copyw" v-if="result_width > 0" @click="copyId('resultwidth')"><img :src="copyimage" /></div>
        </div> :
        <div class="inputcontent">
          <input type="number" min="1" id="resultheight" max="999999" maxlength="6" v-model="result_height"
            placeholder="Result Height" />
          <div class="copyh" v-if="result_height > 0" @click="copyId('resultheight')"><img :src="copyimage" /></div>
        </div>
      </div>
    </div>
    <div class="overlay" v-if="showoverlay"> Copied to clipboard</div>
  </div>
</template>
<script setup>

import { ref, watch } from 'vue'
import copyimage from './assets/copy.png'

const item = ref(0)
const source_width = ref(300)
const source_height = ref(200)
const result_width = ref()
const result_height = ref()

const message = ref("")


watch(source_width, () => {
  result_height.value = parseFloat((result_width.value * source_height.value) / source_width.value)
})

watch(source_height, () => {
  if (parseFloat(source_height.value) < 0) {
    source_height.value = parseFloat(Math.abs(source_height.value))
  }
  if (parseFloat(source_height.value) > 99999) {
    source_height.value = 99999
  }
})

watch(source_width, () => {
  if (parseFloat(source_width.value) < 0) {
    source_width.value = parseFloat(Math.abs(source_width.value))
  }
  if (parseFloat(source_width.value) > 99999) {
    source_width.value = 99999
  }
})

watch(result_width, () => {
  result_height.value = Math.round((result_width.value * source_height.value) / source_width.value)
})

watch(result_height, () => {

  result_width.value =  Math.round((result_height.value * source_width.value) / source_height.value)

})

const showoverlay = ref(false)

const copyId = (id) => {

  const inputField = document.getElementById(id)
  const copyTextToClipboard = async (text) => {
    try {
      await navigator.clipboard.writeText(text)
      console.log(text+' Text copied to clipboard successfully!')

      showoverlay.value=true
      setTimeout(() => {
        showoverlay.value=false
      },1000)

    } catch (error) {
      console.error('Failed to copy text:', error)


    }
  }


  const textToCopy = inputField.value // Getting text from input field
  copyTextToClipboard(textToCopy)  // Copying text from clipboard
  

}


</script>

<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

body{padding: 0;margin: 0;}
.wrapper{position: relative;}
.overlay{position: absolute; width: 100%; height: 100%; top:0; background-color: rgba(0,0,0,0.7); z-index: 8; font-size: 16px; font-weight: bold; display: flex; justify-content: center; align-items: center;}
.inputs {
  display: flex;
  gap: 10px;
  align-items: center;
}

label {
    position: absolute;
    font-size: 12px;
    left: 10px;
    top: 20px;
}
.title{font-size: 12px;
    width: 100%; font-weight: 600;
    padding-left: 10%;}

.inputcontent {
  position: relative;
}
a {color:#fff}
.copyw,
.copyh {
  position: absolute;
    top: 2px;
    right: 2px;
    background-color: #000;
    width: 30px;
    height: 30px;
    font-size: 10px;
    border-radius: 6px;
    text-align: center;
    font-weight: 600;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 7px;
    box-sizing: border-box;
    cursor: pointer;
}

.copyw img,
.copyh img {
  width: 20px;
  filter: invert(1);
  opacity: 0.5;
  cursor: pointer;
}

.copyw:hover img,
.copyh:hover img {
  opacity: 1;
}

.header {
  width: 100%;
  height: 80px;
  background-color: #000;
  text-align: center;
  text-transform: uppercase;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 600;font-size: 16px;
  flex-flow: column;
  

}

.content {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  gap: 10px;
  padding-top: 10px;
}

.wrapper {
  width: 320px;
  height: 250px;
  font-size: 26px;
  background-color: #393c40;
  color: #FFF;
}

input {
  width: 120px;
  height: 60px;
  border-radius: 6px;
  background: #fff;
  border: none;
  text-align: center;
  font-size: 24px;
  color: #7f4f24;
  font-weight: bold;
  padding: 0 20px;
  
}
.imagesource input{
  font-size: 18px;
  padding-right: 0px;
  background: transparent;
  border-bottom: 1px solid #000;
  color: #fff;
  padding: 0px 20px;
}

::placeholder {
  color: #666666;
  opacity: 1; /* Firefox */
  font-size: 12px;
}

::-ms-input-placeholder { /* Edge 12 -18 */
  color: #666666;
  font-size: 12px;
}

</style> 
