<template>
  <div class="hello">
    <div><button @click="preve">上一曲</button><button ref="play" @click="playAudio">播放</button><button @click="play">播放</button><button @click="pause">暂停</button><button @click="next">下一曲</button> <span>进度{{progress}}%</span> 音量<input v-model="volume" type="text"></div>
    <ul>
      <li v-for="(v,k) in srcArr" :key="k" @click="setIndex(k)">{{ v.name}}</li>
    </ul>
    <audio ref='audio' autobuffer="true" :src="src"></audio>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        srcArr: [{
          url: '../../static/test.mp3',
          name: '歌曲一'
        }, {
          url: '../../static/Kalimba.mp3',
          name: '歌曲二'
        },{
          url:'http://file.kuyinyun.com/group2/M00/EF/C0/rBBGelUPiWiACpYhAAQfE72-jHE679.mp3',
          name:'歌曲三'
        },{
          url:'http://file.kuyinyun.com/group2/M00/61/1A/rBBGelcTAZCAcQ4cAAcmErMReH4964.mp3',
          name:'歌曲四'
        },{
          url:'http://file.kuyinyun.com/group1/M00/94/83/rBBGdFYkXFuAYuTRAA2ax4_NmEk387.mp3',
          name:'歌曲五'
        },{
          url:'http://file.kuyinyun.com/group2/M00/84/DF/rBBGelZL7EuAKFL_ABiwByGtPBo792.mp3',
          name:'歌曲六'
        }],
        currIndex: 0,
        currentTime: 0,
        duration: 0,
        timer: null,
        progress: 0,
        volume: 10
      }
    },
    watch: {
      'currIndex': {
        handler() {
          this.$nextTick(() => {
            this.play()
          })
        }
      },
      'volume': {
        handler(val) {
          this.$refs.audio.volume = val / 100
        }
      }
    },
    beforeCreated() {
      this.audio = new window.Audio()
      console.log(this.audio)
    },
    computed: {
      src() {
        return this.srcArr[this.currIndex].url
      }
    },
    methods: {
      setIndex(k) {
        this.$nextTick(() => {
          this.play()
        })
        this.currIndex = k
      },
      play() {
        console.log('123', this.$refs.audio.load())
        this.$refs.audio.addEventListener("canplay",
          () => {
            console.log('加载完成')　　
            this.$refs.audio.play()
            this.duration = this.$refs.audio.duration
          console.log(this.$refs.audio.duration)
          this.timer = setInterval(() => {
            this.currentTime = this.$refs.audio.currentTime
            this.progress = parseInt(this.currentTime / this.duration * 100)
            if (this.currentTime / this.duration === 1) {
              clearInterval(this.timer)
            }
          }, 1000)
          },
          false);
        this.$refs.audio.volume = this.volume / 100
        
        this.$nextTick(() => {
          
        })
      },
      pause() {
        this.$refs.audio.pause()
        clearInterval(this.timer)
      },
      _handleLoaded() {},
      next() {
        if (this.currIndex < this.srcArr.length - 1) {
          this.currIndex++
        } else {
          this.currIndex = 0
        }
      },
      preve() {
        if (this.currIndex > 0) {
          this.currIndex--
        } else {
          this.currIndex = this.srcArr.length - 1
        }
      },
      playAudio() {
        if (this.$refs.audio.paused) {
          this.$refs.play.innerHTML = '暂停'
          this.play()
        } else {
          this.$refs.play.innerHTML = '播放'
          this.pause()
        }
      }
    },
    created() {},
    mounted() {
      // console.log(this.audio)
      // this.audio.addEventListener('loadeddata', this._handleLoaded)
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
