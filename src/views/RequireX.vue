<template>
<v-form v-model="formValid">
    <v-jsonschema-form v-if="schema" :schema="schema" :model="dataObject" :options="options" @error="showError" />
  </v-form>
</template>

<script>
import Vue from 'vue'
import Vuetify from 'vuetify'
import Draggable from 'vuedraggable'
import axios from 'axios'
import VueAxios from 'vue-axios'
import Swatches from 'vue-swatches'

import VJsonschemaForm from '@koumoul/vuetify-jsonschema-form'

Vue.use(Vuetify)
Vue.use(VueAxios, axios)

Vue.component('swatches', Swatches)
Vue.component('draggable', Draggable)

export default {
  components: {VJsonschemaForm},
  data() {
    return {
      schema: {
  "type": "object",
  "properties": {
     "reqType":{ 
      "title":"Choose a type",
      "type": "string",
      "enum": [
      "Restriction",
      "Functional Requirement",
      "Quality Requirement"
      ]
  }},
  "dependencies": {
    "reqType": {
      "oneOf": [
        {
          "$ref": "#/definitions/userInt"
        },
        {
          "$ref": "#/definitions/autoAct"
        },
        {
          "$ref": "#/definitions/extInt"
        }
      ]
    }
  },
  "definitions": {
    "userInt": {
      "title": "User Interface",
      "properties": {
        "type": {
          "const": "userInt"
        },
      }
    },
    "autoAct": {
      "title": "Autonomous Activity",
      "properties": {
        "type": {
          "const": "autoAct"
        },

      }
    },
    "extInt": {
      "title": "External interface",
      "properties": {
        "type": {
          "const": "extInt"
        },
      }
    }
  }
},
      dataObject: {},
      formValid: false,
      options: {
        debug: false,
        disableAll: false,
        autoFoldObjects: true
      }
    }
  },
  methods: {
    showError(err) {
      window.alert(err)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

