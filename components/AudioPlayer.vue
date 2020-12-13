<template>
  <div class="flex">
    <button
      @click="toggle"
      class="focus:outline-none transform transition hover:scale-105 text-gray-600 mr-6"
    >
      <svg
        v-if="!played"
        xmlns="http://www.w3.org/2000/svg"
        class="w-10 h-10"
        fill="currentColor"
        viewBox="0 0 16 16"
      >
        <path
          d="M11.596 8.697l-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393z"
        />
      </svg>
      <svg
        v-else
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        class="w-10 h-10"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        />
      </svg>
    </button>
    <div class="flex flex-col justify-between">
      <div class="h-3 relative rounded-full overflow-hidden w-56">
        <div
          class="w-full h-full absolute"
          :class="left ? 'bg-gray-200' : 'bg-gray-400 opacity-40'"
        ></div>
        <div
          class="transition-all ease-out duration-500 h-full bg-gray-600 relative"
          :style="`width:${percentage}%`"
        ></div>
      </div>
      <div class="flex justify-between text-base">
        <span>
          {{ currentTimeDisplay }}
        </span>
        <span>{{ durationDisplay }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    fileName: {
      type: String
    },
    left: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      audio: new Audio(
        `/assets/audios/${this.fileName}`
      ),
      duration: null,
      durationDisplay: "",
      currentTime: 0,
      currentTimeDisplay: "00:00",
      played: false,
      percentage: 0
    };
  },
  mounted() {
    this.audio.onloadedmetadata = () => {
      this.duration = this.audio.duration;
      this.durationDisplay = this.secondsToTime(this.duration);
    };
    let that = this;

    this.audio.addEventListener("timeupdate", function() {
      that.currentTime = that.audio.currentTime;
      that.currentTimeDisplay = that.secondsToTime(that.audio.currentTime);
      that.updatePercentage();
    });
    this.audio.addEventListener("ended", () => {
      this.currentTime = 0;
      this.currentTimeDisplay = "00:00";
      this.updatePercentage();
      this.audio.pause();
      this.audio.currentTime = 0;
      this.toggle();
    });
  },
  watch: {
    played() {
      this.updatePercentage();
    }
  },
  methods: {
    updatePercentage() {
      this.percentage = (this.currentTime * 100) / this.duration;
    },
    toggle() {
      this.played = !this.played;
      if (this.played) {
        this.audio.play();
      } else {
        this.audio.pause();
        this.audio.currentTime = 0;
      }
    },
    secondsToTime(duration) {
      var sec_num = parseInt(duration, 10);
      var hours = Math.floor(sec_num / 3600);
      var minutes = Math.floor((sec_num - hours * 3600) / 60);
      var seconds = sec_num - hours * 3600 - minutes * 60;

      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (seconds < 10) {
        seconds = "0" + seconds;
      }
      return minutes + ":" + seconds;
    }
  }
};
</script>

<style></style>
