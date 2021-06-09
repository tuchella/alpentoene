<template>
  <div id="audio-player-root">
    <!-- Hide the default audio player -->
    <div>
      <audio style="display: none" ref="player" :id="playerid">
        <source :src="`${publicPath}${url}`" type="audio/wav" />
      </audio>
    </div>

    <div class="w-4/4 px-2 pt-2 mt-4" style="margin: auto">
      <div>
        <div class="space-y-2 flex-grow">
          <div id="pogressbar"
            class="bg-gray-300 dark:bg-gray-800 rounded-full overflow-hidden"
          >
            <div
              class="w-1/2 h-1.5 player-progress"
              role="progressbar"
              aria-valuenow="1456"
              aria-valuemin="0"
              aria-valuemax="4550"
              :style="{ width: playProgress + '%' }" 
            ></div>
          </div>
                      <input
              v-model="playbackTime"
              type="range"
              min="0"
              :max="audioDuration"
              class="slider w-full h-full"
              id="position"
              name="position"
            />

          <div
            class="text-gray-500 dark:text-gray-400 flex justify-between text-sm font-medium tabular-nums"
          >
             <div v-html="elapsedTime()"> 00:00 </div>
              <div >
                <div id="button-div" class="flex-initial pr-3">
          <svg
            @click="toggleAudio()"
            v-show="!isPlaying"
            class="play-button text-gray-400"
            :class="{
              'text-orange-600': audioLoaded,
              'hover:text-orange-400': audioLoaded,
              'cursor-pointer': audioLoaded,
            }"
            xmlns="http://www.w3.org/2000/svg"
            
            width="50" height="50" fill="none">
      <circle class="text-gray-300 dark:text-gray-500" cx="25" cy="25" r="24" stroke="currentColor" stroke-width="1.5" />
      <path d="M 19,16 19,34 33,24 z" fill="currentColor" />
    </svg>
          <svg
            @click="toggleAudio()"
            v-show="isPlaying"
            class="play-button text-orange-400 hover:text-orange-400 cursor-pointer"
            xmlns="http://www.w3.org/2000/svg"
            width="50" height="50" fill="none">
      <circle class="text-gray-300 dark:text-gray-500" cx="25" cy="25" r="24" stroke="currentColor" stroke-width="1.5" />
      <path d="M18 16h4v18h-4V16zM28 16h4v18h-4z" fill="currentColor" />
    </svg>
        </div>

              </div>
                                <div v-html="totalTime()"> 00:00 </div>
          </div>
        </div>
      </div>
      <div id="player-row" class="inline-flex flex-wrap w-full max-w-5xl">
        
      </div>
      <div style="display:none">
        <div id="progress-bar" class="flex-grow">
          <div class="overlay-container relative w-full h-full">

            <!-- Show loading indicator until audio has been loaded -->

            <div
              v-show="!audioLoaded"
              class="w-full absolute top-0 bottom-0 right-0 left-0 px-2 pointer-events-none"
              style="color: #94bcec"
            >
              Loading please wait...
            </div>
            <!--
                            <div
                                v-show="audioLoaded"
                                class="flex w-full justify-between absolute top-0 bottom-0 right-0 left-0 px-2 pointer-events-none items-center"
                            >
                            
                                <span class="text-sm" style="color: #94bcec" v-html="elapsedTime()"> 00:00 </span>

                                <span class="text-sm" style="color: #94bcec" v-html="totalTime()"> 00:00 </span>
                                
                            </div>
                            -->
          </div>
        </div>
      </div>
    </div>

    <!-- outer gray border -->
  </div>
</template>

<script>
//import { mapState } from 'vuex'
export default {
  props: ["url", "playerid"],
  /**
   * playbackTime = local var that syncs to audio.currentTime
   * audioDuration = duration of audio file in seconds
   * isPlaying = boolean (true if audio is playing)
   *
   **/
  data() {
    return {
      playbackTime: 0,
      audioDuration: 100,
      audioLoaded: false,
      isPlaying: false,
      publicPath: 'https://files.inspex.dev/alpentoene/',
    };
  },
  computed: {
    playProgress: function() {
      return (this.playbackTime / this.audioDuration) * 100;
    }
  },
  methods: {
    //Set the range slider max value equal to audio duration
    initSlider() {
      var audio = this.$refs.player;
      if (audio) {
        this.audioDuration = Math.round(audio.duration);
      }
    },
    //Convert audio current time from seconds to min:sec display
    convertTime(seconds) {
      const format = (val) => `0${Math.floor(val)}`.slice(-2);
      //var hours = seconds / 3600;
      var minutes = (seconds % 3600) / 60;
      return [minutes, seconds % 60].map(format).join(":");
    },
    //Show the total duration of audio file
    totalTime() {
      var audio = this.$refs.player;
      if (audio) {
        var seconds = audio.duration;
        return this.convertTime(seconds);
      } else {
        return "00:00";
      }
    },
    //Display the audio time elapsed so far
    elapsedTime() {
      var audio = this.$refs.player;
      if (audio) {
        var seconds = audio.currentTime;
        return this.convertTime(seconds);
      } else {
        return "00:00";
      }
    },
    //Playback listener function runs every 100ms while audio is playing
    playbackListener() {
      var audio = this.$refs.player;
      //Sync local 'playbackTime' var to audio.currentTime and update global state
      this.playbackTime = audio.currentTime;

      //console.log("update: " + audio.currentTime);
      //Add listeners for audio pause and audio end events
      audio.addEventListener("ended", this.endListener);
      audio.addEventListener("pause", this.pauseListener);
    },
    //Function to run when audio is paused by user
    pauseListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    //Function to run when audio play reaches the end of file
    endListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    //Remove listeners after audio play stops
    cleanupListeners() {
      var audio = this.$refs.player;
      audio.removeEventListener("timeupdate", this.playbackListener);
      audio.removeEventListener("ended", this.endListener);
      audio.removeEventListener("pause", this.pauseListener);
      //console.log("All cleaned up!");
    },
    toggleAudio() {
      var audio = this.$refs.player;
      //var audio = document.getElementById("audio-player");
      if (audio.paused) {
        audio.play();
        this.isPlaying = true;
      } else {
        audio.pause();
        this.isPlaying = false;
      }
    },
    initAudio() {
      var audio = this.$refs.player;
      //Wait for audio to load, then run initSlider() to get audio duration and set the max value of our slider
      // "loademetadata" Event https://www.w3schools.com/tags/av_event_loadedmetadata.asp
      audio.addEventListener(
        "loadedmetadata",
        function () {
          this.initSlider();
        }.bind(this)
      );
      // "canplay" HTML Event lets us know audio is ready for play https://www.w3schools.com/tags/av_event_canplay.asp
      audio.addEventListener(
        "canplay",
        function () {
          this.audioLoaded = true;
        }.bind(this)
      );
      //Wait for audio to begin play, then start playback listener function
      this.$watch("isPlaying", function () {
        if (this.isPlaying) {
          var audio = this.$refs.player;
          this.initSlider();
          //console.log("Audio playback started.");
          //prevent starting multiple listeners at the same time
          if (!this.listenerActive) {
            this.listenerActive = true;
            //for a more consistent timeupdate, include freqtimeupdate.js and replace both instances of 'timeupdate' with 'freqtimeupdate'
            audio.addEventListener("timeupdate", this.playbackListener);
          }
        }
      });
      //Update current audio position when user drags progress slider
      this.$watch("playbackTime", function () {
        //var audio = this.$refs.player;
        var diff = Math.abs(this.playbackTime - this.$refs.player.currentTime);

        //Throttle synchronization to prevent infinite loop between playback listener and this watcher
        if (diff > 0.01) {
          this.$refs.player.currentTime = this.playbackTime;
        }
      });
    },
  },
  watch: {
    url: function (newV) {
      const oldAudio = this.$refs.player;
      const newAudio = new Audio(this.publicPath + newV);

      this.pauseListener();
      //oldAudio.pause();
      newAudio.volume = 0.05;
      newAudio.currentTime = oldAudio.currentTime;
      this.$refs.player = newAudio;
      this.initAudio();
      this.toggleAudio();

      const fadeOut = setInterval(function () {
          if (oldAudio.volume > 0.05) {
              oldAudio.volume -= 0.05;
          } else {
              oldAudio.volume = 0.0;
              oldAudio.remove();
              clearInterval(fadeOut);
          }
      }, 50);
      const fadeIn = setInterval(function () {
          if (newAudio.volume < 0.95) {
              newAudio.volume += 0.05;
          } else {
              newAudio.volume = 1.0;
              newAudio.remove();
              clearInterval(fadeIn);
          }
      }, 50);
    },
  },
  mounted: function () {
    // nextTick code will run only after the entire view has been rendered
    this.$nextTick(function () {
      this.initAudio();
    });
  },
};
</script>

<style>
input#position {
  opacity: 0;
  margin-top: -10px;
  height: 10px;
  display: block;
  cursor: pointer;
}
#pogressbar {
  cursor: pointer;
}

.player-progress {
  background-color: #232f66;
}
.dark .player-progress {
  background-color: rgb(238, 91, 83);
}

</style>
