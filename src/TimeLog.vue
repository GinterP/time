<template>
  <div>
    <h4>{{sheetTitle}}</h4>
    <form novalidate @submit.prevent='logIt'>
      <div v-if='saveState !== "saving"'>
        <!-- Tag -->
        <date-without-time label='Tag' :value.sync='tag'></date-without-time>
        <!-- Baustelle -->
        <div class='input-field'>
          <label class="active" for='baustelle'>Baustelle</label>
          <input id='baustelle' type='text' v-model='baustelle'>
        </div>
        <!-- Art -->
        <div class="">
          <select id='art' v-model='art' @change="artChanged()" class="browser-default">
            <option value='Arbeit' selected>Arbeit</option>
            <option value='Lager / Werkstatt'>Lager / Werkstatt</option>
            <option value='Weiterbildung'>Weiterbildung</option>
            <option value='Krank'>Krank</option>
            <option value='Urlaub'>Urlaub</option>
            <option value='Feiertag'>Feiertag</option>
          </select>
          <label for='art'>Art</label>
        </div>
        <!-- Von -->
        <time label='Von' :value.sync='von'></time>
        <!-- Bis -->
        <time label='Bis' :value.sync='bis'></time>
        <!-- Pause -->
        <pause label='Pause' :value.sync='pause'></pause>

      </div>

      <div class='row' v-if='saveState !== "saving" && state === "new"'>
        <input type='submit' class='waves-effect waves-light btn col s12' value='Hinzufügen'/>
      </div>
      <div class="row" v-if='saveState !== "saving" && state === "edit"'>
        <input type='submit' class='waves-effect waves-light btn col s6' value='Speichern'/>
        <button class="btn grey waves-effect waves-light col s6" v-on:click="setState('new', null)">Abbrechen
        </button>
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
      <div>
        <a href='#' @click.prevent='refreshRecords' title='refresh'>&#x21bb; Refresh</a>
      </div>

      <div>
        <ul class="collection">
          <li v-for='record in lastRecords' class="collection-item">
            <span class="title">{{record[2]}} ({{record[3]}})</span>
            <p>{{record[1]}}<br>
              {{record[4]}} - {{record[5]}} <br>
              {{record[6]}} Minuten Pause
            </p>
            <a href='javascript:void(0)' v-on:click="setState('edit', record)"><i class="material-icons">edit</i></a>
            <!--
            <a href='javascript:void(0)' v-on:click="removeRow(record[0])"><i class="material-icons">remove_circle_outline</i></a>
            -->
            <span class="new badge secondary content" data-badge-caption="">{{record[0]}}</span>
          </li>
        </ul>
      </div>

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
  /* eslint-disable prefer-template,no-alert,no-undef */

  // This is the heart of the application. This file may not be the prettiest.
  import appModel from './lib/appModel.js';
  import {getError, logTime, resetTime, updateTime, removeRow, getSheetTitle} from './lib/goog.js';
  import {convertDateToSheetsDateOnlyString} from './lib/dateUtils.js';
  import getLastRecordsForComponent from './lib/getLastRecordsForComponent.js';
  import getSpreadsheetIdFromComponentRoute from './lib/getSpreadsheetIdFromComponentRoute.js';
  import DateWithoutTime from './DateWithoutTime.vue';
  import Time from './Time.vue';
  import Pause from './Pause.vue';

  export default {
    data() {
      return {
        state: 'new',
        editingId: -1,
        recordsState: 'loading',
        lastRecords: [],
        tag: '',
        baustelle: '',
        art: '',
        von: '',
        bis: '',
        pause: '',
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
      DateWithoutTime,
      Time,
      Pause
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
      setState(state, editingItem) {
        this.state = state;

        if (editingItem) {
          $('html, body').animate({scrollTop: 0});
          // window.scrollTo(0, 0);
          this.editingId = editingItem[0];
          let d = editingItem[1].split('.');
          this.tag = d[2] + '-' + d[1] + '-' + d[0];
          this.baustelle = editingItem[2];
          this.art = editingItem[3];
          this.von = resetTime(editingItem[4]);
          this.bis = resetTime(editingItem[5]);
          this.pause = editingItem[6];
          this.error = '';
        } else {
          this.resetState();
        }
      },

      refreshRecords() {
        getLastRecordsForComponent(this);
      },

      resetState() {
        this.tag = '';
        this.baustelle = '';
        this.art = 'Arbeit';
        this.von = '';
        this.bis = '';
        this.pause = '';

        this.state = 'new';
        this.saveState = 'done';
        this.error = '';
      },

      isNotEmpty() {
        // if (this.art === 'Arbeit' || this.art === 'Lager / Werkstatt'
        // || this.art === 'Weiterbildung') {
        return this.tag && this.baustelle && this.art &&
          this.von && this.von !== 'Von' &&
          this.bis && this.bis !== 'Bis' &&
          this.pause;
        // }
      },

      removeRow(removeId) {
        if (confirm('Eintrag wirklich löschen?')) {
          if (removeId !== undefined) {
            const spreadsheetId = getSpreadsheetIdFromComponentRoute(this);
            this.saveState = 'saving';

            removeRow(spreadsheetId, removeId).then(() => {
              this.saveState = 'done';
              this.refreshRecords();
            });
          }
        }
      },
      artChanged() {
        console.log('changed');
      },

      logIt() {
        const tag = convertDateToSheetsDateOnlyString(this.tag);
        const spreadsheetId = getSpreadsheetIdFromComponentRoute(this);
        if (this.isNotEmpty()) {
          if (this.state === 'new') {
            this.saveState = 'saving';

            logTime(spreadsheetId,
              this.lastRecords.length + 1,
              tag,
              this.baustelle,
              this.art,
              this.von,
              this.bis,
              this.pause)
              .then(() => {
                // TODO: This is not very reliable.
                this.lastRecords.unshift([
                  this.lastRecords.length + 1,
                  tag,
                  this.baustelle,
                  this.art,
                  this.von,
                  this.bis,
                  this.pause
                ]);

                // reset everything here
                this.resetState();
              }, response => {
                this.saveState = 'error';
                this.error = getError(response);
              });
          } else {
            this.saveState = 'saving';

            updateTime(spreadsheetId,
              this.editingId,
              tag,
              this.baustelle,
              this.art,
              this.von,
              this.bis,
              this.pause)
              .then(() => {
                this.refreshRecords();
                this.resetState();
              }, response => {
                this.saveState = 'error';
                this.error = getError(response);
              });
          }
        }
      },
    },
  };
</script>
