<template>
  <div class="sound-icon">
    <img :src="iconPath" :alt="altText">
    <div ref="slider" class="volume-slider"></div>
    <audio ref="audio" loop>
      <source :src="soundPath" type="audio/mpeg">
    </audio>
  </div>
</template>

<script>
import noUiSlider from 'nouislider';
import 'nouislider/dist/nouislider.css';

export default {
props: {
  iconPath: {
    type: String,
    required: true
  },
  soundPath: {
    type: String,
    required: true
  },
  altText: {
    type: String,
    default: 'Sound Icon'
  }
},
data() {
  return {
    volume: 0,
  };
},
mounted() {
  noUiSlider.create(this.$refs.slider, {
    start: [0],
    connect: [true, false],
    range: {
      'min': [0],
      'max': [100]
    }
  });

  this.$refs.slider.noUiSlider.on('update', (values) => {
    this.volume = parseFloat(values[0]);
  });
},
watch: {
  volume(newVolume) {
    this.setVolume(newVolume);
  }
},
methods: {
  setVolume(volume) {
    const audio = this.$refs.audio;
    audio.volume = volume / 100;
    
    if (audio.paused && volume > 0) {
      audio.play().catch(error => {
        console.error("Error playing audio:", error);
      });
      this.$emit('user-interaction');
    } else if (volume <= 0 && !audio.paused) {
      audio.pause();
    }
  },
  stopSound() {
    this.$refs.audio.pause();
    this.$refs.audio.currentTime = 0;
    this.$refs.slider.noUiSlider.set(0);
  },
      attemptVideoPlayback() {
    const videoElement = this.$refs.backgroundVideo;
    if (videoElement.paused) {
      videoElement.play().catch(error => {
        console.error("Error playing video:", error);
      });
    }
  },
}
};
</script>
  
  <style scoped>
  /* Additional styles to improve the look of the slider and layout. */
  .sound-icon {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 1rem;
  }
  

  @media (max-width: 768px) {
    .sound-icon {
    margin: 0rem;
  }
  }

  img {
    width: 130px;
    height: 130px;
    margin-bottom: 10px;
    opacity: 0.79;
  }

  .volume-slider {
  width: 100px;
    }
 .noUi-connect {
  background: #77A4A0;
    }

  </style>
  