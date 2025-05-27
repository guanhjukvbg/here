<template>
  <div class="screen-center">
    <div class="nailong" ref="container">
      <img src="/src/assets/奶龙.svg" alt="" class="nailong-body">
      
      <div class="eye left-eye" ref="leftEye">
        <img src="/src/assets/眼珠.svg" alt="" class="pupil" :style="leftEyeStyle">
      </div>
      
      <div class="eye right-eye" ref="rightEye">
        <img src="/src/assets/眼珠.svg" alt="" class="pupil" :style="rightEyeStyle">
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const container = ref(null)
const leftEye = ref(null)
const rightEye = ref(null)
const mousePosition = ref({ x: 0, y: 0 })
const leftEyeStyle = ref({})
const rightEyeStyle = ref({})


const updateEyePosition = (eyeElement, eyeStyle) => {
  
  const containerRect = container.value.getBoundingClientRect()
  const eyeRect = eyeElement.value.getBoundingClientRect()
  
  const eyeCenterX = eyeRect.left + eyeRect.width / 2 - containerRect.left
  const eyeCenterY = eyeRect.top + eyeRect.height / 2 - containerRect.top
  
  const mouseX = mousePosition.value.x - containerRect.left
  const mouseY = mousePosition.value.y - containerRect.top
  
  const angle = Math.atan2(mouseY - eyeCenterY, mouseX - eyeCenterX)
  const maxDistance = eyeRect.width / 4
  const distance = Math.min(maxDistance)
  
  const offsetX = Math.cos(angle) * distance
  const offsetY = Math.sin(angle) * distance
  
  eyeStyle.value = {
    transform: `translate(${offsetX}px, ${offsetY}px)`,
    transition: 'transform 0.1s ease-out'
  }
}

const handleMouseMove = (e) => {
  mousePosition.value = { x: e.clientX, y: e.clientY }
  updateEyePosition(leftEye, leftEyeStyle)
  updateEyePosition(rightEye, rightEyeStyle)
  
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
})
</script>

<style lang="scss">
.screen-center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; 
  width: 100vw;  
}

.nailong {
  position: relative;
  width: 810px;
  height: 800px;
  &-body {
    width: 100%;
    height: 100%;
  }
  
  .eye {
    position: absolute;
    border-radius: 50%;
    overflow: hidden;
    
    &.left-eye {
      left: 24.1%;
      top: 26.6%;
      width: 88.09px;
      height: 103.59px;
      transform: rotate(12.89deg);
      background: #B2B0B5;
    }
    
    &.right-eye {
      left: 53.5%;
      top: 26.5%;
      width: 108.13px;
      height: 102.92px;
      transform: rotate(-0.96deg);
      background: #D9CDC8;
    }
    
    .pupil {
      position: absolute;
      width: 82px;
      height: 93px;
      bottom: 0;
      right: 0;
      transform: translate(-50%, -50%);
    }
  }
}
</style>