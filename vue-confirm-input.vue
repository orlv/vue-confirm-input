<template id="vue-confirm-input-template">
    <span>
        <span class="text-button p-0" :class="{'text-button-red': !busy }" @click="click">{{ buttonText }}</span>
        <span class="vue-confirm-input-form" v-if="busy" @click="cancel">
            <span @click.stop class="vue-confirm-input-main">
                <span>{{ title }}</span>
                <input v-model="inputValue" type="text" class="ml-1" :title="title" autofocus ref="confirmInput"
                       @keydown.enter="callConfirm"/>
                <span class="text-button text-button-green" @click="callConfirm">{{ confirmText }}</span>
            </span>
        </span>
    </span>
</template>

<script>
  import 'rlv-styles/styles.css'

  export default {
    template: '#vue-confirm-input-template',

    data () {
      return {
        busy: false,
        inputValue: ''
      }
    },

    props: ['callback', 'text', 'title', 'confirm', 'value'],

    created () {
      this.inputValue = this.value || ''
    },

    watch: {
      value (value) {
        this.inputValue = value || ''
      }
    },

    computed: {
      buttonText () {
        return this.busy ? 'Cancel' : this.text
      },

      confirmText () {
        return this.confirm ? this.confirm : 'OK'
      }
    },

    methods: {
      callConfirm () {
        this.busy = false
        const value = this.inputValue
        this.inputValue = ''
        this.callback(value)
      },

      cancel () {
        this.busy = false
        this.inputValue = this.value || ''
      },

      click () {
        if (this.busy) {
          this.cancel()
        } else {
          this.busy = true
          this.$nextTick(() => this.$refs.confirmInput.focus())
        }
      }
    }
  }
</script>

<style>
    .vue-confirm-input-form {
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(249, 249, 249, 0.89);
    }

    .vue-confirm-input-main {
        background-color: #ffffff;
        padding: 1em;
        border-radius: 0.3em;
        border: 1px solid #e2e2e2;
    }
</style>
