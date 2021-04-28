<template>
  <v-row>
    <v-col class="pa-0">
      <svg v-if="enable" version="1.1"      baseProfile="full"
        x="0" y ="0"
        :width="width" :height="height" :viewBox="viewBox"
        xmlns="http://www.w3.org/2000/svg">

        <g fill="none" stroke="black" stroke-width="2">
          <rect :x="x" :y="y" :width="w" :height="h" />
        </g>
        <!--<text x="0" y="35" font-family="Verdana" font-size="35">lv:{{level}}</text>-->
      </svg>
    </v-col>
  </v-row>
</template>

<script>
export default {
  components: {
  },
  data() {
    return {
      width: 800,
      height: 600,
      viewBox: "0 0 800 600",
      x: 100 ,
      y: 200,
      w: 100,
      h: 80 ,
      enable: false,
      params: {
        1 : {min: 1, max:2, div: 6},
        2 : {min: 1, max:3, div: 11},
        3 : {min: 1, max:3, div: 21},
      },
      level: 1,
    }
  },
  created() {
    this.$nuxt.$on('set_level', (level => {
      this.setLevel(level);
    }));
    this.$nuxt.$emit('get_level');
  },
  methods: {
    generate() {
      let array = this.divide(this.params[this.level]);
      let answer;
      let select;
      let length = (this.width > this.height) ? this.height : this.width;

      do {
        let propotion = array[Math.floor(Math.random() * array.length)];

        this.x = Math.floor(this.width * Math.random() * 0.7);
        this.y = Math.floor(this.height * Math.random())* 0.7;

        if (Math.random() < 0.5) {
          this.w = Math.floor(length * Math.random() * 0.4);
          this.h = Math.floor(this.w * propotion);
          answer = '1 : ' + propotion; 
          select = array.map(value => '1 : ' + value);
        } else {
          this.h = Math.floor(length * Math.random() * 0.4);
          this.w = Math.floor(this.h * propotion);
          answer = '' + propotion + ' : 1'; 
          select = array.map(value => '' + value + ' : 1');
        }
        console.log("x:", this.x, ", y:", this.y, ", w:", this.w, ", h:", this.h, ", answer:", answer, ", select:", select);
      } while (  
        this.x + this.w > this.width
        || this.y + this.h > this.height
        || this.w < length / 8
        || this.h < length / 8
      );

      this.$nuxt.$emit('init', {items: select, answer: answer, levels: Object.keys(this.params)});
    },
    handleResize() {
      // resizeのたびにこいつが発火するので、ここでやりたいことをやる
      if (process.browser){
        this.width = window.innerWidth;
        this.height = window.innerHeight - 64 - 36;
        this.viewBox = [0, 0, this.width, this.height].join(" ");
      }
    },
    divide(param) {
      // param = {min: 最小の数, max: 最大の数, div: 分割数}
      let result = [];
      let step = (param.max - param.min) / (param.div - 1);
      for(let i = 0; i < param.div; i++) result.push(Math.floor(100 * (param.min + step * i)) / 100);

      return result;
    },
    setLevel(level) {
      console.log("setlevel");
      if (level == 0) {
        this.level = Object.keys(this.params)[0];        
      } else {
        this.level = level;      
      }
      this.generate();
    }
  },
  mounted() {
    if (process.browser){
      this.handleResize();
      this.enable = true;
      window.addEventListener('resize', this.handleResize);
    }
    console.log("lv:", this.level);

  },
  beforeDestroy() {
    if (process.browser){
      window.removeEventListener('resize', this.handleResize);
    }
    this.$nuxt.$off('set_level');
  }
}
</script>
