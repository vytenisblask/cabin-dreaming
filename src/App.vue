<template>
  <div class="app-container">
    <img src="@/assets/np_cabin.svg" alt="Cabin" class="cabin-image">
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
  height: 100%;
  background-color: #FFDAB9;
}

.app-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100vh;
  padding: 1rem; 
}

.cabin-image {
  flex: 0 0 50%; 
  max-width: 100%;
  max-height: 400px;
}

.icons-container {
  display: flex;
  flex-direction: row;
  gap: 0.2rem;
  flex: 0 0 50%;  
  align-items: center;
  padding: 0.1rem; 
}


@media (max-width: 768px) {
  .icons-container {
    flex-direction: column;
    align-items: center;
  }
  .cabin-image {
    display: none;
}
}


</style>
