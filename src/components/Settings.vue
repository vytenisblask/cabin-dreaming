<template>
    <div class="settings-wrapper">
      <img src="@/assets/np_settings.svg" alt="Settings Icon" :style="{ transform: `rotate(${rotateDegree}deg)` }" @click="toggleMenu" class="settings-icon" />
      <div v-if="menuVisible" class="settings-menu">
        <div class="timer-container">
            <label>
                <input type="checkbox" :checked="timerEnabled" @change="onTimerEnabledChange">
                stop after: 
            </label>
            <input type="number" :value="timerMinutes" @input="onTimerMinutesChange" min="1" :disabled="!timerEnabled">
            minutes
        </div>
      </div>
    </div>
</template>
  
<script>
export default {
name: 'AppSettings',
props: {
    timerEnabled: {
    type: Boolean,
    required: true,
    },
    timerMinutes: {
    type: Number,
    required: true,
    },
},
data() {
    return {
    menuVisible: false,
    rotateDegree: 0,
    };
},
methods: {
    onTimerEnabledChange(event) {
    this.$emit('update:timerEnabled', event.target.checked);
    },
    onTimerMinutesChange(event) {
    this.$emit('update:timerMinutes', parseFloat(event.target.value));
    },
    toggleMenu() {
    this.menuVisible = !this.menuVisible;
    this.rotateDegree += 180;
    },
},
};
</script>
  

<style scoped>
.timer-container {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    z-index: 4;
    font-size: 1rem;
}

input[type="number"] {
    width: 60px;
}

.settings-wrapper {
    position: fixed;
    top: 10px;
    left: 10px;
    z-index: 1000;
}

.settings-icon {
    width: 40px;
    cursor: pointer;
    transition: transform 1.3s ease;
}

.settings-menu {
    width: 300px;
    max-height: 500px;
    background-color: #FFDAB9;
    border-radius: 8px;
    overflow: hidden;
    transition: max-height 1.3s ease;
}

.timer-container {
    padding: 1rem;
}
</style>