<template>
  <div class="timer-container" @click="handleClick">
    <div class="time-box">
      {{ formattedTime }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 0, // 0.1秒単位
      intervalId: null,
      started: false,
      bgColor: '#007BFF', // 初期背景色（青）
      clickTimestamps: [],
      audio: Object.assign(new Audio('deathHinana.mp3'), { volume: 0.3}),
      altAudio: Object.assign(new Audio('ohennjikoito.mp3'), { volume: 0.1}),
      pyakoitoAudio: Object.assign(new Audio('pyakoito.mp3'), { volume: 0.1}),
      enelAudio: Object.assign(new Audio('enelHinana.mp3'), { volume: 0.1}),
    };
  },
  mounted() {
    this.audio = new Audio('deathHinana.mp3'); // ← ファイルを読み込む
    this.altAudio = new Audio('ohennjikoito.mp3');
  },
  computed: {
    formattedTime() {
      return (this.time / 10).toFixed(1);
    }
  },
  methods: {
    handleClick() {
      const now = Date.now();
      this.clickTimestamps = this.clickTimestamps.filter(ts => now - ts <= 1000);
      this.clickTimestamps.push(now);

      if (this.clickTimestamps.length >= 3) {
        this.resetToInitialState(); // ← 初期状態に戻す
        this.clickTimestamps = [];
        return;
      }

      if ((this.bgColor === '#007BFF' || this.bgColor === 'yellow')) {
        if (this.altAudio) {
          this.altAudio.volume = 0.3;
          this.altAudio.currentTime = 0;
          this.altAudio.play().catch(err => {
            console.warn('再生がブロックされました:', err);
        });
      }
    }

       if (this.bgColor === 'red' && this.time < 300) {
        if (this.audio) {
          this.audio.volume = 0.3;
          this.audio.currentTime = 0;
          this.audio.play().catch(err => {
            console.warn('再生がブロックされました:', err);
          });
        }
      }

      if (this.time >= 300 && this.bgColor === 'red') {
    if (this.altAudio) {
      this.altAudio.volume = 0.3;
      this.altAudio.currentTime = 0;
      this.altAudio.play().catch(err => {
        console.warn('音声再生失敗:', err);
      });
    }
  }

      if (!this.started) {
        this.startTimer();
      }

      if (this.started && this.time < 300) {
        if (this.bgColor === '#007BFF') {
          this.bgColor = 'yellow';
        } else if (this.bgColor === 'yellow') {
          this.bgColor = 'red';
        }
      }

      if (this.time >= 300) {
        this.resetTimer(true); // ← 通常リセット
      }
    },
    startTimer() {
      this.started = true;
      this.intervalId = setInterval(() => {
        this.time++;

         if (this.time === 285 && this.pyakoitoAudio) {
          this.pyakoitoAudio.volume = 0.3;
      this.pyakoitoAudio.currentTime = 0;
      this.pyakoitoAudio.play().catch(err => {
        console.warn('pyakoito 再生失敗:', err);
      });
    }

          if (this.time === 205 && this.pyakoitoAudio) {
            this.pyakoitoAudio.volume = 0.3;
      this.pyakoitoAudio.currentTime = 0;
      this.pyakoitoAudio.play().catch(err => {
        console.warn('pyakoito 再生失敗:', err);
      });
    }

    if (this.time === 300 && this.enelAudio) {
      this.enelAudio.volume = 0.3;
      this.enelAudio.currentTime = 0;
      this.enelAudio.play().catch(err => {
        console.warn('enelHinana 再生失敗:', err);
      });
    }

      }, 100);
    },
    resetTimer(shouldRestart) {
      clearInterval(this.intervalId);
      this.time = 0;
      this.bgColor = 'yellow';
      this.started = false;
      if (shouldRestart) {
        this.startTimer();
      }
    },
    resetToInitialState() {
      clearInterval(this.intervalId);
      this.time = 0;
      this.bgColor = '#007BFF'; // 青に戻す
      this.started = false;
    }
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  }
};
</script>

<style scoped>
.timer-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: v-bind(bgColor);
  user-select: none;
  cursor: pointer;
  transition: background-color 0.01s ease;
}

.time-box {
  background-color: white;
  padding: 40px 60px;
  border-radius: 20px;
  font-size: 6rem;
  font-weight: 900;
  color: black;
  font-family: 'Courier New', Courier, monospace;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}
</style>
