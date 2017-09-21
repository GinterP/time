<template>
  <div>
    <h4>{{sheetTitle}}</h4>
    <form novalidate @submit.prevent='logIt'>
      <date-without-time label='Start' :value.sync='day'></date-without-time>
      <div class='input-field'>
        <label for='what'>What?</label>
        <input id='what' type='text' v-model='what'>
      </div>

      <div class='row' v-if='saveState !== "saving"'>
        <input type='submit' class='waves-effect waves-light btn col s12' value='Log time'/>
      </div>
    </form>

    <div v-if='saveState === "saving"'>
      <h4>Saving...</h4>
      <div class='progress'>
        <div class='indeterminate'></div>
      </div>
    </div>

    <div v-if='saveState === "error"' class='card-panel red-text'>
      <h4>Error...</h4>
      <pre><code>{{error}}</code></pre>
      <div>
        Refresh the page maybe?
      </div>
    </div>

    <div v-if='recordsState === "loaded"'>
      <table>
        <thead>
        <tr>
          <th data-field='nr'>Nr</th>
          <th data-field='tag'>Tag</th>
          <th data-field='baustelle'>Baustelle</th>
          <th data-field='art'>Art</th>
          <th data-field='von'>Von</th>
          <th data-field='bis'>Bis</th>
          <th data-field='pause'>Pause</th>
          <th data-field='gesamt'>Gesamt
            <a href='#' @click.prevent='refreshRecords' class='right' title='refresh'>&#x21bb;</a>
          </th>
        </tr>
        </thead>
        <tbody>
        {{lastRecords}}
        <tr v-for='record in lastRecords'>
          <td>{{record[0]}}</td>
          <td>{{record[1]}}</td>
          <td>{{record[2]}}</td>
          <td>{{record[3]}}</td>
          <td>{{record[4]}}</td>
          <td>{{record[5]}}</td>
          <td>{{record[6]}}</td>
          <td>{{record[7]}}</td>
        </tr>
        </tbody>
      </table>
      <div class='fixed-action-btn' style='bottom: 12px; right: 12px;'>
        <a class='btn-floating btn-small red' :href='editLink' title='Edit records...' target='_blank'>
          <i class='small material-icons'>mode_edit</i>
        </a>
      </div>
    </div>

    <div v-if='recordsState === "loading"'>
      <h4>Loading records...</h4>
      <div class='progress'>
        <div class='indeterminate'></div>
      </div>
    </div>
  </div>
</template>

<script>
  // This is the heart of the application. This file may not be the prettiest.
  import appModel from './lib/appModel.js';
  import {getError, logTime, getSheetTitle} from './lib/goog.js';
  import {convertDateToSheetsDateString} from './lib/dateUtils.js';
  import getLastRecordsForComponent from './lib/getLastRecordsForComponent.js';
  import getSpreadsheetIdFromComponentRoute from './lib/getSpreadsheetIdFromComponentRoute.js';
  import DateWithoutTime from './DateWithoutTime.vue';

  export default {
    data() {
      return {
        recordsState: 'loading',
        lastRecords: [],
        day: '',
        saveState: '',
        sheetTitle: ''
      };
    },
    computed: {
      /**
       * Provides a Google Docs link to edit a spreadsheet
       */
      editLink() {
        const sheetId = getSpreadsheetIdFromComponentRoute(this);
        return `https://docs.google.com/spreadsheets/d/${sheetId}/edit`;
      }
    },
    components: {
      DateWithoutTime
    },
    route: {
      data() {
        appModel.pageName = 'Loading data...';

        getLastRecordsForComponent(this).then(() => {
          const sheetId = getSpreadsheetIdFromComponentRoute(this);
          getSheetTitle(sheetId, title => {
            appModel.pageName = title;
            this.sheetTitle = title;
          });
        })
      }
    },

    methods: {
      refreshRecords() {
        getLastRecordsForComponent(this);
      },

      logIt() {
        this.saveState = 'saving';

        const day = convertDateToSheetsDateString(this.day);
        const spreadsheetId = getSpreadsheetIdFromComponentRoute(this);

        logTime(spreadsheetId, day)
          .then(() => {
            // TODO: This is not very reliable.
            this.lastRecords.unshift([day]);

            // reset everything here
            this.day = '';


            this.saveState = 'done';
            this.error = '';
          }, response => {
            this.saveState = 'error';
            this.error = getError(response);
          });
      },
    },
  };
</script>
