<template>
  <div>
    <label>{{label}}</label>
    <input v-el:date type='time' v-model='value' data-input>
  </div>
</template>
<script>
  export default {
    props: ['label', 'value'],

    ready() {
    },

    detached() {
    },

    methods: {
    },

    watch: {
      value(newValue) {
        let a = newValue;
        a = +a;
      },
    },
  };

</script>
