<template>
  <div class="timer-container">
    <div class="time">{{ formattedTime }}</div>
    <div class="buttons">
      <button @click="startTimer">Start</button>
      <button @click="pauseTimer">Pause</button>
      <button @click="resetTimer">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 0,
      intervalId: null
    };
  },
  computed: {
    formattedTime() {
      const minutes = String(Math.floor(this.time / 60)).padStart(2, '0');
      const seconds = String(this.time % 60).padStart(2, '0');
      return `${minutes}:${seconds}`;
    }
  },
  methods: {
    startTimer() {
      if (this.intervalId) return;
      this.intervalId = setInterval(() => {
        this.time++;
      }, 1000);
    },
    pauseTimer() {
      clearInterval(this.intervalId);
      this.intervalId = null;
    },
    resetTimer() {
      this.pauseTimer();
      this.time = 0;
    }
  },
  beforeUnmount() {
    this.pauseTimer();
  }
};
</script>

<style scoped>
.timer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #007BFF; /* 青色背景 */
  color: white;
  font-size: 3rem;
  text-align: center;
}

.buttons {
  margin-top: 20px;
}

.buttons button {
  margin: 0 10px;
  padding: 10px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.buttons button:hover {
  background-color: rgba(255, 255, 255, 0.2);
}
</style>
