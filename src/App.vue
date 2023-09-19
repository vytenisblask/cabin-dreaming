<template>
  <div id="app">
    <div class="icons-container">
      <SoundIcon :iconPath="require('@/assets/np_fire.svg')" :soundPath="require('@/assets/fire_cracklin.mp3')" altText="Fire Icon" />
      <SoundIcon :iconPath="require('@/assets/np_river.svg')" :soundPath="require('@/assets/river_flow.mp3')" altText="River Icon" />
      <SoundIcon :iconPath="require('@/assets/np_wind.svg')" :soundPath="require('@/assets/wind_husl.mp3')" altText="Wind Icon" />
      <SoundIcon :iconPath="require('@/assets/np_birds.svg')" :soundPath="require('@/assets/birds_chrp.mp3')" altText="Birds Icon" />
    </div>
  </div>
</template>

<script>
import SoundIcon from '@/components/SoundIcon.vue';

export default {
  name: 'App',
  components: {
    SoundIcon
  }
}
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  font-family: 'Arial', sans-serif;
  height: 100vh;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #FFDAB9;
  height: 100%;
}

.icons-container {
  display: flex;
  gap: 2rem;
}
</style>
