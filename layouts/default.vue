<template>
  <v-app>
    <v-app-bar
      app
    >
      <v-toolbar-title v-text="title" />
      <v-select
        v-model="level"
        :items="levels"
        class="pt-5 pl-4"
        style="max-width: 80px;"
        label="level"
        @input="handleLevel"
        dense
      ></v-select>

      <v-spacer />
      <span v-if="hide">
        <v-select
          v-model="value"
          :items="items"
          style="max-width: 80px;"
          @input="handleSelect"
          dense
        ></v-select>
      </span>
      <span v-else>
        {{answer}}
        <v-btn @click="reload">つぎ</v-btn>
      </span>
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <nuxt @init="init" @get_level="getLevel" :key="resetKey" ref="page"/>
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      fixed: false,
      hide: true,
      title: 'propotion proto',
      answer: '',
      items: [],
      value: '',
      resetKey: 0,
      level: 0,
      levels: [1],
    }
  },
  created() {
    // emitで発火させたイベント名にする
    this.$nuxt.$on('init', this.init);
    this.$nuxt.$on('get_level', this.getLevel);
  },
  mounted() {
    this.$nuxt.$emit('set_level', this.level);
  },
  methods: {
    init(param) {
      this.items = param.items;
      this.value = param.items[0];
      this.answer = param.answer;
      this.levels = param.levels;
      if (this.level == 0) {
        this.level = param.levels[0];
      }
    },
    toggle() {
      this.hide = !this.hide;
    },
    handleSelect() {
      this.toggle();
      if (this.value == this.answer){
        this.answer = '⭕ ' + this.answer;
      }else{
        this.answer = '❌ ' + this.value + ' → ' + this.answer;
      }
    },
    handleLevel() {
      this.resetKey += 1;
    },
    reload() {
      this.toggle();
      this.resetKey += 1;
    },
    getLevel() {
      this.$nuxt.$emit('set_level', this.level);
    },
  },
  beforeDestroy() {
    this.$nuxt.$off('init');
    this.$nuxt.$on('get_level');
  }
}
</script>
