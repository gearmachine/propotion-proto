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
      const a_max = 5;
      const a_min = 1;
      const b_max = 20;
      const b_min = 0;
      do {
        this.x = Math.floor(this.width * Math.random());
        this.y = Math.floor(this.height * Math.random());
        let a = 50 * (Math.floor( 2 * (Math.random() * (a_max + 1 - a_min)) + a_min)); // 100 ~ 700, 50 刻み
        let b = 1.0 + 0.1 * (Math.floor( (Math.random() * (b_max + 1 - b_min)) + b_min));
        if (Math.random() < 0.5) {
          this.w = a;
          this.h = Math.floor(b * a);
        } else {
          this.w = Math.floor(b * a);
          this.h = a; 
        }
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
      return ''
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
