<template>
  <li class="iii">
    <span>{{ position }}</span>
    <span>{{ description }}</span>
    <span>{{ convert(localCurrentTime) }}</span>
    <span>{{ state }}</span>
    <button @click="$emit('on-delete')" class="btn">
      <span class="mdi mdi-delete"></span>
    </button>
    <button
      v-if="state"
      @click="$emit('on-stop')" class="btn">
      <span class="mdi mdi-stop"></span>
    </button>
    <button
      v-if="!state"
      @click="$emit('on-play')" class="btn">
      <span class="mdi mdi-play"></span>
    </button>
  </li>
</template>

<script>
  export default {
    data() {
      return {
        localCurrentTime: this.currentTime,
        timer: null,
      };
    },
    props: {
      state: Boolean,
      description: String,
      time: Number,
      position: Number,
      currentTime: Number,
      completed: Boolean
    },
    mounted() {
      this.startTimer()
    },
    destroyed() {
    },
    methods: {
      convert(s) {
        let h, m;
        m = Math.floor(s / 60);
        s = s % 60;
        h = Math.floor(m / 60);
        m = m % 60;
        h = h % 24;
        return h + ':' + m + ':' + s;
      },
      startTimer() {
        if (this.state) {
          this.timer = setInterval(() => {
            this.localCurrentTime++;
          }, 1000)
        }
      },
      stopTimer() {
        clearInterval(this.timer)
      },
    },
    watch: {
      currentTime(time) {
        if (time === 0) {
          this.stopTimer()
        }
      },
      state(state) {
        if (state) {
          this.startTimer();
        } else {
          this.stopTimer();
        }
      }
    },
  };
</script>
