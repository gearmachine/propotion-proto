<template>
  <v-app>
    <v-app-bar
      app
    >
      <v-toolbar-title v-text="title" />
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
        <nuxt @init="init" :key="resetKey"/>
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
    }
  },
  created() {
    // emitで発火させたイベント名にする
    this.$nuxt.$on('init', this.init)
  },
  methods: {
    init(param) {
      this.items = param.items;
      this.value = param.items[0];
      this.answer = param.answer;
    },
    toggle() {
      this.hide = !this.hide;
    },
    handleSelect() {
      console.log('handleSelect');
      this.toggle();
      if (this.value == this.answer){
        this.answer = '⭕ ' + this.answer;
      }else{
        this.answer = '❌ ' + this.value + ' → ' + this.answer;
      }
    },
    reload() {
      this.toggle();
      this.resetKey += 1;
    }
  }
}
</script>
