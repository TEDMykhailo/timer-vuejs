<template>
  <div class="second-screen">
    <div class="text-header">
      <span class="text-header-why">Why&nbsp;</span>
      <span class="text-header-why-do">do we use it?</span>
    </div>
    <div class="text-info">
      <span>
        This sounded nonsense to Alice, so she said nothing, but set off at once toward the Red Queen. To her surprise, she lost sight of her in a moment.
      </span>
    </div>
    <div class="list">
      <div class="create">
        <create-timer @on-new-timer="addTimer($event)"/>
      </div>
      <div class="m-top-25">
        <div class="list-group">
          <timer
            v-for="(timer, index) in timers"
            :someTimer="timer"
            :description="timer.description"
            :time="timer.now"
            :position="index + 1"
            :state="timer.state"
            :currentTime="timer.currentTime"
            @on-delete="deleteTimer(timer)"
            @on-stop="stopTimer(timer)"
            @on-play="playTimer(timer)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Timer from "./Timer.vue";
  import CreateTimer from "./CreateTimer.vue";

  export default {
    data() {
      return {
        timers: [],
      };
    },
    methods: {
      addTimer(newTimer) {
        let now = new Date().valueOf();
        let currentTime = 1;
        this.timers.push({
          description: newTimer,
          now: now,
          state: true,
          currentTime: currentTime,
          dirty: false
        });
        document.cookie = JSON.stringify(this.timers);
      },
      deleteTimer(deletedTimer) {
        this.timers = this.timers.filter(timer => timer !== deletedTimer);
        document.cookie = JSON.stringify(this.timers);
      },
      stopTimer(stoppedTimer) {
        let realNow = new Date().valueOf();
        let a, v;

        this.timers.forEach(function (timer) {
          if (timer === stoppedTimer) {
            timer.state = !timer.state;
            if (!timer.dirty && !timer.state) {
              timer.dirty = !timer.dirty;
            }

            timer.currentTime = Math.floor((realNow - timer.now) / 1000);
            a = Math.floor((realNow - timer.now) / 1000);
            v = a - timer.currentTime;
            timer.now = timer.now - v;
            timer.currentTime = Math.floor((realNow - timer.now) / 1000);
          }
        });
        document.cookie = JSON.stringify(this.timers);
      },
      playTimer(playedTimer) {
        this.timers.forEach(function (timer) {
          if (timer === playedTimer) {
            timer.state = !timer.state;
          }
        });
        document.cookie = JSON.stringify(this.timers);
      },
    },
    components: {Timer, CreateTimer},
    mounted() {
      if (document.cookie.length > 0) {
        let realNow = new Date().valueOf();
        this.timers = JSON.parse(document.cookie);
        this.timers.forEach(function (timer) {
          if (timer.state) {
            timer.currentTime = Math.floor((realNow - timer.now) / 1000);
          }
        })
      }
    },
  };
</script>
