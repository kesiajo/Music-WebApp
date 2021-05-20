<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} - <span> {{current.artist}}</span></h2>
      <div class= "ctrl">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
      </section>
      <section class="playlist">
        <br>
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key= "song.src" @click="play(song)" :class="(
          song.src == current.src) ? 'song playing' : 'song'">
          {{song.title}} - {{song.artist}}
          </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index : 0,
      isPlaying: false,
      songs: [
        {
          title: 'Believe in Me',
          artist: 'Demi Lovatp',
          src: require('./assets/Believe-in-me.mp3')
        },
        {
          title: 'A Whole New World',
          artist: 'Zhavia Ward',
          src: require('./assets/A-Whole-New-World.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.curent = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index ++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current  = this.songs[this.index];
        this.play(this.current);
      }.bind(this))
      this.isPlaying =  true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current  = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current  = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.play();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #d4a5a5;
  color: #ffff;
}
main {
  width: 100%;
  max-width: 760px;
  margin: 0 auto;
}

.song-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.ctrl {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #cc2e50;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 15px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #ff5858;
}

</style>
