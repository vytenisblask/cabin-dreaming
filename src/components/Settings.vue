<template>
    <div class="settings-wrapper">
        
        <img src="@/assets/np_settings.svg" alt="Settings Icon" :style="{ transform: `rotate(${rotateDegree}deg)` }" @click="toggleMenu" class="settings-icon" />
        <div v-if="menuVisible" class="settings-menu">
        <div class="timer-container">
            <div class="clocks-set">
            <div class="custom-checkbox">
                <input type="checkbox" id="timerCheckbox" :checked="timerEnabled" @change="onTimerEnabledChange" hidden>
                <label for="timerCheckbox" class="checkbox-label"></label>
            </div>
            <label for="timerCheckbox">
                stop after: 
            </label>
            <input type="number" :value="timerMinutes" @input="onTimerMinutesChange" min="1" :disabled="!timerEnabled">
            minutes
            </div>
            <div v-if="timerEnabled && remainingTime" class="countdown">
                {{ Math.floor(remainingTime / 60) }}m {{ remainingTime % 60 }}s left
            </div>
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
            required: true
        },
        timerMinutes: {
            type: Number,
            required: true
        }
    },
    data() {
        return {
            menuVisible: false,
            rotateDegree: 0,
            remainingTime: 0,
            countdownInterval: null
        };
    },
    watch: {
        timerEnabled(val) {
            if (val) {
                this.startCountdown();
            } else {
                this.stopCountdown();
            }
        },
        timerMinutes(val) {
            if (this.timerEnabled) {
                this.remainingTime = val * 60;
            }
        }
    },
    methods: {
        onTimerEnabledChange(event) {
            this.$emit('update:timerEnabled', event.target.checked);
            if (event.target.checked) {
                this.remainingTime = this.timerMinutes * 60;
            }
        },
        onTimerMinutesChange(event) {
            this.$emit('update:timerMinutes', parseFloat(event.target.value));
        },
        toggleMenu() {
            this.menuVisible = !this.menuVisible;
            this.rotateDegree += 180;
        },
        startCountdown() {
        if (this.countdownInterval) {
            clearInterval(this.countdownInterval);
        }
        this.countdownInterval = setInterval(() => {
            this.remainingTime--;
            if (this.remainingTime <= 0) {
                this.stopCountdown();
                this.$emit('update:timerEnabled', false);
                this.$emit('timerFinished');
            }
        }, 1000);
        },
        stopCountdown() {
            if (this.countdownInterval) {
                clearInterval(this.countdownInterval);
                this.countdownInterval = null;
                this.remainingTime = 0;
            }
        }
    },
    beforeUnmount() {
        if (this.countdownInterval) {
            clearInterval(this.countdownInterval);
        }
    }
};
</script>

  

<style scoped>
.timer-container {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    z-index: 4;
    font-size: 1rem;
    flex-direction: column;
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

.custom-checkbox {
    position: relative;
    display: inline-block;
    width: 20px;
    height: 20px;
    vertical-align: middle;
    margin-right: 5px;
}

.checkbox-label {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #ccc;
    border-radius: 4px;
    cursor: pointer;
}

.checkbox-label:after {
    content: "";
    position: absolute;
    left: 6px;
    top: 2px;
    width: 5px;
    height: 10px;
    border: solid #4e7766;
    border-width: 0 3px 3px 0;
    transform: rotate(45deg);
    display: none;
}

input[type="checkbox"]:checked + .checkbox-label:after {
    display: block;
}

input[type="number"] {
    width: 50px;
}

.countdown {
    margin-top: 5px;
    font-size: 0.9rem;
    color: #333;
}
</style>