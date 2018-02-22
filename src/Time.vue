<template>
  <div class="">
    <select id="label" disabled='{{disabled}}' v-model='value' class="browser-default">
      <option value='' disabled selected>{{label}}</option>
      <option value='06:00'>06:00 Uhr</option>
      <option value='06:15'>06:15 Uhr</option>
      <option value='06:30'>06:30 Uhr</option>
      <option value='06:45'>06:45 Uhr</option>
      <option value='07:00'>07:00 Uhr</option>
      <option value='07:15'>07:15 Uhr</option>
      <option value='07:30'>07:30 Uhr</option>
      <option value='07:45'>07:45 Uhr</option>
      <option value='08:00'>08:00 Uhr</option>
      <option value='08:15'>08:15 Uhr</option>
      <option value='08:30'>08:30 Uhr</option>
      <option value='08:45'>08:45 Uhr</option>
      <option value='09:00'>09:00 Uhr</option>
      <option value='09:15'>09:15 Uhr</option>
      <option value='09:30'>09:30 Uhr</option>
      <option value='09:45'>09:45 Uhr</option>
      <option value='10:00'>10:00 Uhr</option>
      <option value='10:15'>10:15 Uhr</option>
      <option value='10:30'>10:30 Uhr</option>
      <option value='10:45'>10:45 Uhr</option>
      <option value='11:00'>11:00 Uhr</option>
      <option value='11:15'>11:15 Uhr</option>
      <option value='11:30'>11:30 Uhr</option>
      <option value='11:45'>11:45 Uhr</option>
      <option value='12:00'>12:00 Uhr</option>
      <option value='12:15'>12:15 Uhr</option>
      <option value='12:30'>12:30 Uhr</option>
      <option value='12:45'>12:45 Uhr</option>
      <option value='13:00'>13:00 Uhr</option>
      <option value='13:15'>13:15 Uhr</option>
      <option value='13:30'>13:30 Uhr</option>
      <option value='13:45'>13:45 Uhr</option>
      <option value='14:00'>14:00 Uhr</option>
      <option value='14:15'>14:15 Uhr</option>
      <option value='14:30'>14:30 Uhr</option>
      <option value='14:45'>14:45 Uhr</option>
      <option value='15:00'>15:00 Uhr</option>
      <option value='15:15'>15:15 Uhr</option>
      <option value='15:30'>15:30 Uhr</option>
      <option value='15:45'>15:45 Uhr</option>
      <option value='16:00'>16:00 Uhr</option>
      <option value='16:15'>16:15 Uhr</option>
      <option value='16:30'>16:30 Uhr</option>
      <option value='16:45'>16:45 Uhr</option>
      <option value='17:00'>17:00 Uhr</option>
      <option value='17:15'>17:15 Uhr</option>
      <option value='17:30'>17:30 Uhr</option>
      <option value='17:45'>17:45 Uhr</option>
      <option value='18:00'>18:00 Uhr</option>
      <option value='18:15'>18:15 Uhr</option>
      <option value='18:30'>18:30 Uhr</option>
      <option value='18:45'>18:45 Uhr</option>
      <option value='19:00'>19:00 Uhr</option>
      <option value='19:15'>19:15 Uhr</option>
      <option value='19:30'>19:30 Uhr</option>
      <option value='19:45'>19:45 Uhr</option>
      <option value='20:00'>20:00 Uhr</option>
      <option value='20:15'>20:15 Uhr</option>
    </select>
    <label for="label">{{label}}</label>
  </div>
</template>
<script>
  export default {
    props: ['label', 'value', 'disabled'],

    ready() {
    },

    detached() {
    },

    methods: {},
  };

</script>
