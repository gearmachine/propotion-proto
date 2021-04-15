<template>
  <div id="app">
    <svg version="1.1"
     baseProfile="full"
     :width="width" :height="height"
     xmlns="http://www.w3.org/2000/svg">

      <rect :x="x" :y="y" :width="w" :height="h" stroke="black" fill="white" stroke-width="5" />
      
    </svg>
    <button v-on:click="changeProportion">変更</button>
    <div>(x, y) = ({{ x + ', ' + y }})</div>
    <div>(w, h) = ({{ w + ', ' + h }}) &nbsp;&nbsp; {{ calcProportion() }}</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      width: 800,
      height: 600,
      x: 10,
      y: 10,
      w: 150,
      h: 150,
    }
  },
  created() {
    this.changeProportion();
  },
  methods: {
    changeProportion: function() {
      const max = 7;
      const min = 1;
      do {
        this.x = Math.floor(this.width * Math.random());
        this.y = Math.floor(this.height * Math.random());
        this.w = 50 * (Math.floor( 2 * (Math.random() * (max + 1 - min)) + min));
        this.h = 50 * (Math.floor( 2 * (Math.random() * (max + 1 - min)) + min));
      } while ( this.x > this.width * 0.7 
        || this.y > this.height * 0.7 
        || this.x + this.w > this.width
        || this.y + this.h > this.height
        || this.w < 100
        || this.h < 100
        || this.w * 4 < this.h
        || this.w > this.h * 4
        || this.w == this.h
      )
    },
    calcProportion: function() {
      let gcd = this.gcd(this.w, this.h);
      return '' + this.w / gcd + ':' + this.h / gcd + ' = '
       + ((this.w > this.h) ? '' + Math.round(this.w/this.h * 100) / 100 + ':1' : '1:' + Math.round(this.h / this.w * 100) / 100);
    },
    gcd: function(m, n) {
      // 最大公約数を求める
      if (n == 0) return m;
      return this.gcd(n, m % n);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
