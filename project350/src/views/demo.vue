<template>
    <div>
      <label for="upload-btn">Upload Image:</label>
      <input id="upload-btn" type="file" @change="onFileChange" accept="image/*">
      <br><br>
      <img :src="imageUrl" v-if="imageUrl" alt="Uploaded Image">
    </div>
  </template>
  
  <script>
  import { ref } from 'vue'
  
  export default {
    setup() {
      const imageUrl = ref(null)
  
      const onFileChange = (e) => {
        const file = e.target.files[0]
        if (file && file.type.startsWith('image/')) {
          const reader = new FileReader()
          reader.readAsDataURL(file)
          reader.onload = () => {
            imageUrl.value = reader.result
          }
        } else {
          imageUrl.value = null
        }
      }
  
      return {
        imageUrl,
        onFileChange,
      }
    },
  }
  </script>