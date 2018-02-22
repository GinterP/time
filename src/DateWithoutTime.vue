<template>
  <div>
    <label>{{label}} <a href='#' v-clap.prevent='setNow()'>Heute</a></label>
    <input v-el:date type='date' v-model='value' data-input>
  </div>
</template>
<script>
import {getNow} from './lib/dateUtils.js';

export default {
  props: ['label', 'value'],
  methods: {
    setNow() {
      this.value = getNow().split('T')[0];
    },
  },

};
</script>
