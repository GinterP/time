<template>
  <div class="">
    <select id="label" :disabled='disabled' v-model='value' class="browser-default">
      <option value='' disabled selected>{{label}}</option>
      <option value='0'>0 Minuten</option>
      <option value='5'>5 Minuten</option>
      <option value='10'>10 Minuten</option>
      <option value='15'>15 Minuten</option>
      <option value='20'>20 Minuten</option>
      <option value='25'>25 Minuten</option>
      <option value='30'>30 Minuten</option>
      <option value='35'>35 Minuten</option>
      <option value='40'>40 Minuten</option>
      <option value='45'>45 Minuten</option>
      <option value='50'>50 Minuten</option>
      <option value='55'>55 Minuten</option>
      <option value='60'>60 Minuten</option>
      <option value='65'>65 Minuten</option>
      <option value='70'>70 Minuten</option>
      <option value='75'>75 Minuten</option>
      <option value='80'>80 Minuten</option>
      <option value='85'>85 Minuten</option>
      <option value='90'>90 Minuten</option>
      <option value='95'>95 Minuten</option>
      <option value='100'>100 Minuten</option>
      <option value='105'>105 Minuten</option>
      <option value='110'>110 Minuten</option>
      <option value='115'>115 Minuten</option>
      <option value='120'>120 Minuten</option>
    </select>
    <label for="label">{{label}}</label>
  </div>
</template>
<script>
  export default {
    props: ['label', 'value', 'disabled']
  };

</script>

