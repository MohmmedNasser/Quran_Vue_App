<template>
  <div id="app">
    <header>
      <h1> تطبيق القرآن </h1>
    </header>
    <main>
      <section>
        <h2 class="quran-title">
          {{ current.title }} - <span>{{ current.recited }}</span>
        </h2>
        <div class="controls">
          <button class="next" @click="next">التالي</button>
          <button class="play" @click="play" v-if="!isPlaying">تشغيل</button>
          <button class="pause" @click="pause" v-else>ايقاف</button>
          <button class="prev" @click="prev">السابق</button>

        </div>
      </section>

      <section class="playlist">
        <h3>قائمة السور</h3>
        <button v-for="suwar in quran" :key="suwar.src" @click="play(suwar)" :class="(suwar.src == current.src) ? 'quran playing' : 'quran'">
          {{ suwar.title }} - {{ suwar.recited }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      quran: [
        {
          title: "الكوثر",
          recited : "ماهر المعيقلي",
          src: require("./assets/al-kauther.mp3")
        },
        {
          title: "الناس",
          recited : "ماهر المعيقلي",
          src: require("./assets/an-nas.mp3")
        },
        {
          title: "النصر",
          recited : "ماهر المعيقلي",
          src: require("./assets/an-nasr.mp3")
        },
        {
          title: "الكافرون",
          recited : "ماهر المعيقلي",
          src: require("./assets/al-kafiroon.mp3")
        },
      ],
      player: new Audio()
    }
  },

  methods: {
    play(suwar) {
      if(typeof suwar.src != "undefined") {
        this.current = suwar;
        this.player.src = this.current.src
      }
      this.player.play();
      
      this.player.addEventListener("ended", () => {
        this.index++;
        if(this.index > this.quran.length - 1 ) {
          this.index = 0;
        }
        this.current = this.quran[this.index];
        this.play(this.current);
      })
      this.isPlaying  = true;
    },

    pause () {
      this.player.pause();
      this.isPlaying  = false;
    },

    next () {
      this.index++;
      if(this.index > this.quran.length - 1) {
        this.index = 0;
      }

      this.current = this.quran[this.index];
      this.play(this.current)
    },

    prev () {
      this.index--;
      if(this.index < 0) {
        this.index = this.quran.length - 1;
      }

      this.current = this.quran[this.index];
      this.play(this.current)
    },
  },


  created () {
    this.current = this.quran[this.index];
    this.player.src = this.current.src;
    // this.player.play();
  }
}



</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
  font-family: 'Cairo', sans-serif;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #212121;
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.quran-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.quran-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
  font-family: 'Cairo', sans-serif;
  font-size: 14px;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}
.next, .prev {
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .quran {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .quran:hover {
  color: #FF5858;
}
.playlist .quran.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>
