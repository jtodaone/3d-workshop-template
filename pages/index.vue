<template>
  <audio src="/audio.wav" ref="audioElement" />
  
  <TresCanvas :windowSize="true" shadows>
    <TresPerspectiveCamera :args="[45, 1, 0.1, 1000]" :position="[20, 20, -20]" :look-at="[0, 0, 0]" ref="camera">
      <TresAudioListener v-if="audioEnabled" ref="listener" />
    </TresPerspectiveCamera>
    <OrbitControls v-if="camera" :camera="camera" @start="enableAudio" />
    
    <TresScene>
      <!-- Scene Components -->
    </TresScene>
    
    <TresDirectionalLight :position="[-4, 8, 4]" :intensity="5" cast-shadow />
    <TresAudio v-if="listener" ref="audio" :args="[listener]" />
    <TresAudioAnalyser v-if="audio" ref="analyser" :args="[audio, 64]" />
  </TresCanvas>
</template>

<script setup lang="ts">
const { onLoop } = useRenderLoop()

const camera = shallowRef(null)
const audioEnabled = ref(false)
const audio = shallowRef(null)
const listener = shallowRef(null)
const analyser = shallowRef(null)

const audioElement = ref(null)

function enableAudio() {
  if (audioEnabled.value) return
  
  console.log('starting audio')
  audioEnabled.value = true
  
  console.log(listener)
  
  nextTick().then(() => {
    audio.value.setMediaElementSource(audioElement.value)
    audioElement.value.play()
  })
}

onLoop(({delta, elapsed}) => {
  if (!analyser.value) return
})
</script>

<style>
html {
  background: #45d8c2;
}
</style>