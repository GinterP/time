<template>
  <div>
    <h3>Neue Tabelle erstellen</h3>
    <form @submit.prevent='create'>
      <div class='input-field'>
        <label for='name'>Arbeiter (Vorname):</label>
        <input id='name' type='text' v-model='name' autofocus>
      </div>
      <input type='submit' class='waves-effect waves-light btn col s12' value='Neue Datei anlegen' v-if='status === ""'/>
    </form>

    <div v-if='status === "saving"'>
      <h4>Saving {{name}}...</h4>
      <div class='progress'>
          <div class='indeterminate'></div>
      </div>
    </div>

    <div v-if='error' class='card-panel red-text'>
      <h4>Error...</h4>
      <pre><code>{{error}}</code></pre>
      <div>
        Refresh the page maybe?
      </div>
    </div>

  </div>
</template>

<script>
import {getError, createSpreadsheet} from './lib/goog.js';

export default {
  data() {
    return {
      name: '',
      status: '',
      error: ''
    };
  },

  methods: {
    create() {
      this.status = 'saving';
      createSpreadsheet(this.name).then(result => {
        this.status = '';
        const {spreadsheetId} = result;
        this.$router.go(`/time-log/${spreadsheetId}`);
      }, response => {
        this.error = getError(response)
      });
    },
  },
};
</script>
