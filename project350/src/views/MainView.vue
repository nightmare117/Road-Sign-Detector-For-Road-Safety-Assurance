<script setup>
import {ref} from 'vue'
import axios from 'axios'
const imagePointer = ref(null)
const previewImage = ref(null)
const noImg = ref(true)
const imageUrl = ref(null)
const prediction = ref(null)
const imgUploadButtonClicked = () => {
    // console.log('oh no')
    imagePointer.value.click()
}

const imageUploaded = (e) => {
    const file = e.target.files[0]
      if (file && file.type.startsWith('image/')) {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = () => {
          imageUrl.value = reader.result
        }
        const formData = new FormData()
        formData.append('image', file)

        axios.post('http://127.0.0.1:5000/predict', formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      }).then(response => {
        console.log(response.data)
        prediction.value = response.data
      }).catch(error => {
        console.error(error)
      })
        noImg.value = false
      } else {
        imageUrl.value = null
        noImg.value = true
        prediction.value = null
      }
}
</script>
<template>
    <div class="mainPage">
        <div class="videocontainer">
            <video class="videoBack" autoplay loop muted plays-inline>
            <source src="../assets/autonomousvehicle.mp4" type="video/mp4">
            </video>
        </div>
        <div class="mainContainer">
            <div class="imageBox">
                <input type="file" id="imagefile" @change="imageUploaded" ref="imagePointer" accept="image" hidden>
                <img :src="imageUrl" v-if="imageUrl" class="imgupload" alt="uploaded image"/>
                <img v-if="noImg" src="../assets/image2.png" alt="uploadimage"/>
                <h7 v-if="noImg">Please upload a image</h7>
                <h7 class="prediction-class" v-else>{{prediction}}</h7>
            </div>
            <div class="uploadImageButton" @click="imgUploadButtonClicked">
                <p>UPLOAD</p>
                <img src="../assets/upload.png" alt="upload"/>
            </div>
        </div>
    </div>
</template>

<style>
.prediction-class{
    color: rgba(255, 255, 255, 0.747);
    margin-top: 0 !important;
    margin-left: 15px;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.imageBox .imgupload{
    border: 5px ridge rgba(8, 90, 156, 0.774);
    align-self: center;
    justify-self: center;
    max-width: 70%;
    max-height: 70%;
    width: auto;
    height: auto;
    min-width: 200px;
    min-height: 200px;
    margin-bottom: 50px;
    background: white;
}
.uploadImageButton:hover p{
    /* margin-left: 50%; */
    font-size: 17px;
    padding-left: 15px;
    padding-right: 15px;

}
.uploadImageButton:hover{
    /* margin-left: 50%; */
    padding-left: 15px;
    padding-right: 15px;

}
.uploadImageButton img{
    height: 15px;
    width: 15px;
    margin-left: 10px;
}
.uploadImageButton p{
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.uploadImageButton{
    /* margin-left: 50%; */
    padding-left: 10px;
    padding-right: 10px;
    margin-top: 15px;
    border-radius: 5px;
    background: white;
    height: 30px;
    width: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: rgba(14, 30, 37, 0.12) 0px 2px 4px 0px, rgba(14, 30, 37, 0.32) 0px 2px 16px 0px;
    cursor: pointer;
}
.imageBox h7{
    color: rgba(255, 255, 255, 0.747);
    margin-top: 35px;
    margin-left: 15px;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.imageBox img{
    margin-top: 50px;
    height: 150px;
    width: 150px;
}
.imageBox{
    height: 60%;
    width: 60%;
    background: rgba(0, 0, 0, 0.425);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
.mainContainer{
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px, rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px, rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px;
}
.videocontainer{
    
    /* overflow: hidden;
    width: 100vw;
    height: 100vh; */
    /* background: red; */
    position: absolute;
    right: 0;
    bottom: 0;
    z-index: -1;
    filter: blur(6px);
    -webkit-filter: blur(6px);
}
.videoBack{
    width: 100%;
    height: 100%;
}
.mainPage{
    height: 100vh;
    width: 100vw;
}
</style>