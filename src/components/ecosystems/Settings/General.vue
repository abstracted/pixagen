<template>
  <div>
    <v-list two-line subheader>
      <v-subheader class="light-text">KEYWORD OPTIONS</v-subheader>
      <v-divider></v-divider>
      <v-list-tile>
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Keywords</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">Amount of keywords to generate</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action class="kw-selection">
          <v-select
          solo
          dense
          v-model="keywordAmount"
          :items="kwAmountChoices"
          :label="String(keywordAmount)"
        ></v-select>
        </v-list-tile-action>
      </v-list-tile>
      </app-tile-slider>
      <v-divider inset></v-divider>
      <v-list-tile>
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Single Only</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">All keywords forced to one word</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-list-tile-action-text>
            <v-checkbox color="primary" v-model="single"></v-checkbox>
          </v-list-tile-action-text>
        </v-list-tile-action>
      </v-list-tile>
      <v-divider inset></v-divider>
      <v-list-tile>
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Safe Search</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">Filter results for adult content</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-list-tile-action-text>
            <v-checkbox color="primary" v-model="safeMode"></v-checkbox>
          </v-list-tile-action-text>
        </v-list-tile-action>
      </v-list-tile>
      <v-divider inset></v-divider>
      <v-list-tile @click="nav('s1')">
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Blacklist</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">Filter results for specific words</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-list-tile-action-text>
            <v-icon color="primary">
              keyboard_arrow_right
            </v-icon>
          </v-list-tile-action-text>
        </v-list-tile-action>
      </v-list-tile>
<!-- <v-divider inset></v-divider>
        <v-list-tile @click="nav('s2')">
          <v-list-tile-content>
            <v-list-tile-title>Translate</v-list-tile-title>
            <v-list-tile-sub-title>Translate language of the keywords</v-list-tile-sub-title>
          </v-list-tile-content>
          <v-list-tile-action>
            <v-list-tile-action-text>
              <v-icon color="primary">
                keyboard_arrow_right
              </v-icon>
            </v-list-tile-action-text>
          </v-list-tile-action>
      </v-list-tile> -->
      <v-divider></v-divider>
      <v-subheader class="light-text">CATEGORY OPTIONS</v-subheader>
      <v-divider></v-divider>
      <v-list-tile @click="nav('s3')">
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Categories</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">Toggle categories used for searching</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-list-tile-action-text>
            <v-icon color="primary">
              keyboard_arrow_right
            </v-icon>
          </v-list-tile-action-text>
        </v-list-tile-action>
      </v-list-tile>
      <v-divider inset></v-divider>
      <v-list-tile>
        <v-list-tile-content>
          <v-list-tile-title class="darker-text">Unique Categories</v-list-tile-title>
          <v-list-tile-sub-title class="light-text">Disable duplicate categories usage</v-list-tile-sub-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-list-tile-action-text>
            <v-checkbox color="primary" v-model="unique"></v-checkbox>
          </v-list-tile-action-text>
        </v-list-tile-action>
      </v-list-tile>
    </v-list>
  </div>
</template>
<script>
  import { routerHelpers } from '../../../router/routerHelpers.js'
  export default {
    mixins: [routerHelpers],
    methods: {
      slider (active) {
        if (active) {
          document.querySelector('.kw-counter').classList.add('sliding')
        } else {
          document.querySelector('.kw-counter').classList.remove('sliding')
        }
      }
    },
    computed: {
      settings () {
        return this.$store.getters.getSettings
      },
      safeMode: {
        get () {
          return this.settings.keywords.safeMode
        },
        set () {
          this.$store.dispatch('toggleSafemode')
        }
      },
      keywordAmount: {
        get () {
          return this.settings.keywords.amount
        },
        set (payload) {
          this.$store.dispatch('setKeywordAmount', payload)
        }
      },
      single: {
        get () {
          return this.settings.keywords.single
        },
        set () {
          this.$store.dispatch('toggleSingle')
        }
      },
      unique: {
        get () {
          return this.settings.categories.unique
        },
        set () {
          this.$store.dispatch('toggleUnique')
        }
      },
      kwAmountChoices () {
        let max = this.$store.getters.getSettings.keywords.max
        let choices = []
        for (let i = 0; i < max; i++) {
          choices[i] = i + 1
        }
        return choices
      }
    },
    mounted () {
      this.$store.dispatch('setSettingsNav', { title: 'Settings', back: 'b0' })
    }
  }
</script>
<style scoped>
  .kw-selection {
    max-width: 50px;
  }
  .kw-counter {
    margin-right: 8px;
  }
  .sliding {
    font-size: 14pt;
    font-weight: bold;
  }
</style>