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
    'type': 'object',
    properties: {
      'name': { 'type': 'string' },
      acceptTC: {
        title: 'Accept terms and conditions',
        type: 'boolean'
      }
    },
    dependencies: {
      acceptTC: {
        oneOf: [{$ref: '#/definitions/creditCardPayment'}, {$ref: '#/definitions/paypalPayment'}]
      }
    },
    definitions: {
      'creditCardPayment': {
        title: 'Credit card payment',
        'properties': {
          type: {const: 'creditcardpayment'},
          'credit_card': { 'type': 'number' }
        },
        'required': ['name'],
        'dependencies': {
          'credit_card': {
            'properties': {
              'billing_address': { 'type': 'string' }
            },
            'required': ['billing_address']
          }
        }
      },
      'paypalPayment': {
        title: 'Paypal payment',
        'properties': {
          type: {const: 'paypalpayment'},
          'paypal account': { 'type': 'string' }
        },
        'required': ['account']
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

