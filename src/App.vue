<template>
  <div class="video-container">
    <video class="background-video" ref="backgroundVideo" playsinline autoplay muted loop>
      <source src="@/assets/cabin_video_bck.mp4" type="video/mp4">
      Your browser does not support the HTML5 video tag. Please upgrade your browser.
    </video>
  <div class="app-container">
    <div class="icons-container">
      <SoundIcon ref="soundIcon1" :iconPath="require('@/assets/np_fire.png')" :soundPath="require('@/assets/fire_cracklin.mp3')" altText="Fire Icon" />
      <SoundIcon ref="soundIcon2" :iconPath="require('@/assets/np_river.png')" :soundPath="require('@/assets/river_flow.mp3')" altText="River Icon" />
      <SoundIcon ref="soundIcon3" :iconPath="require('@/assets/np_wind.png')" :soundPath="require('@/assets/wind_husl.mp3')" altText="Wind Icon" />
      <SoundIcon ref="soundIcon4" :iconPath="require('@/assets/np_birds.png')" :soundPath="require('@/assets/birds_chrp.mp3')" altText="Birds Icon" />
    </div>
    <div class="timer-container">
      <label>
        <input type="checkbox" v-model="timerEnabled" @change="toggleTimer">
        stop after: 
      </label>
      <input type="number" v-model.number="timerMinutes" min="1" @input="updateTimer" :disabled="!timerEnabled">
      minutes
    </div>
  </div>
  </div>
</template>

<!-- App.vue -->
<script>
import SoundIcon from '@/components/SoundIcon.vue';

export default {
  name: 'App',
  components: {
    SoundIcon,
  },
  data() {
    return {
      timerEnabled: false,
      timerMinutes: 0,
      timerId: null,
      playbackSpeed: 0.9,
    };
  },
  methods: {
    stopAllSounds() {
      this.$refs.soundIcon1.stopSound();
      this.$refs.soundIcon2.stopSound();
      this.$refs.soundIcon3.stopSound();
      this.$refs.soundIcon4.stopSound();
    },
    toggleTimer() {
      if (this.timerEnabled) {
        this.startTimer();
      } else {
        this.clearTimer();
      }
    },
    updateTimer() {
      this.clearTimer();
      this.startTimer();
    },
    startTimer() {
      if (this.timerMinutes > 0) {
        this.timerId = setTimeout(() => {
          this.stopAllSounds();
          this.timerEnabled = false;
        }, this.timerMinutes * 60 * 1000);
      }
    },
    clearTimer() {
      if (this.timerId) {
        clearTimeout(this.timerId);
        this.timerId = null;
      }
    },
},
mounted() {
    this.$refs.backgroundVideo.playbackRate = this.playbackSpeed;
  }
};
</script>


<style>
body, html {
  margin: 0;
  padding: 0;
  font-family: 'Arial', sans-serif;
  height: 100%;
  background-color: #FFDAB9;
}

.video-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.background-video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 1;
  opacity: 0.8;
}

.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 95vh;
  padding: 1rem; 
  z-index: 2;
}

.icons-container {
  display: flex;
  flex-direction: row;
  gap: 0.2rem;
  flex: 0 0 50%;  
  align-items: center;
  padding: 0.1rem; 
  z-index: 5;
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

.timer-container {
  position: fixed;
  bottom: 1rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  z-index: 4;
  font-size: 1rem;
}

input[type="number"] {
  width: 60px;
}

</style>
