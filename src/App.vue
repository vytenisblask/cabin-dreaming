<template>
  <div class="video-container">
    <video class="background-video" ref="backgroundVideo" playsinline autoplay muted loop>
      <source src="@/assets/cabin_video_bck.mp4" type="video/mp4">
      Your browser does not support the HTML5 video tag. Please upgrade your browser.
    </video>
    <div class="app-container">
      <div class="icons-container">
        <SoundIcon ref="soundIcon1" :iconPath="require('@/assets/np_fire.png')" :soundPath="require('@/assets/fire_cracklin_3.mp3')" altText="Fire Icon" />
        <SoundIcon ref="soundIcon2" :iconPath="require('@/assets/np_river.png')" :soundPath="require('@/assets/river_flow_3.mp3')" altText="River Icon" />
        <SoundIcon ref="soundIcon3" :iconPath="require('@/assets/np_wind.png')" :soundPath="require('@/assets/wind_husl_3.mp3')" altText="Wind Icon" />
        <SoundIcon ref="soundIcon4" :iconPath="require('@/assets/np_birds.png')" :soundPath="require('@/assets/birds_chrp_2.mp3')" altText="Birds Icon" />
        <SoundIcon ref="soundIcon5" :iconPath="require('@/assets/np_thunder.svg')" :soundPath="require('@/assets/lightning_crack_3.mp3')" altText="Thunder Icon" />
      </div>
      <div class="settings-container">
        <AppSettings 
        v-model:timerEnabled="timerEnabled" 
        v-model:timerMinutes="timerMinutes"
        @timerFinished="stopAllSounds" />
      </div>
    </div>
  </div>
</template>

<script>
import SoundIcon from '@/components/SoundIcon.vue';
import AppSettings from '@/components/Settings.vue';

export default {
  name: 'App',
  components: {
    SoundIcon,
    AppSettings,
  },
  data() {
    return {
      timerEnabled: false,
      timerMinutes: 5,
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
  height: 100vh;
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
  justify-content: center;
}

@media (max-width: 768px) {
  .icons-container {
    flex-wrap: wrap;
  }

  .icons-container > * {
    flex: 0 0 50%;
    box-sizing: border-box;
  }
  .cabin-image {
    display: none;
  }
  .icons-container {
    gap: 2rem;
  }
  .app-container {
    padding: 0;
    height: 100vh;
  }
}

</style>