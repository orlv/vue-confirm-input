<template>
    <span>
        <span class="show-button" :class="{'show-button-idle': !busy }" @click.stop="click">{{ buttonText }}</span>
        <span class="container" v-if="busy" @click="cancel">
            <span @click.stop class="vue-confirm-input-main">
                <span>{{ title }}</span>
                <input v-model.trim="inputValue"
                       type="text"
                       class="confirm-input"
                       :title="title"
                       autofocus
                       ref="confirmInput"
                       @keydown.enter="callConfirm"/>
                <span class="confirm-button" @click.stop="callConfirm">{{ confirmText }}</span>
            </span>
        </span>
    </span>
</template>

<script>

export default {
  data () {
    return {
      busy: false,
      inputValue: ''
    }
  },

  props: ['callback', 'text', 'title', 'confirm', 'value'],

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
        this.inputValue = this.value || ''
        this.$nextTick(() => this.$refs.confirmInput.focus())
      }
    }
  }
}
</script>

<style scoped>
.container {
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

.container > span {
    background-color: #ffffff;
    padding: 1em;
    border-radius: 0.3em;
    border: 1px solid #e2e2e2;
}

.show-button {
    padding: 0;
    cursor: pointer;
    font-weight: 700;
    text-align: center;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

.show-button-idle {
    color: #e0412e;
}

.show-button-idle:hover {
    color: #d8422f;
}

.confirm-button {
    padding: 0 0.6em;
    cursor: pointer;
    font-weight: 700;
    text-align: center;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    user-select: none;
    color: #70ab6c;
}

.confirm-button:hover {
    color: #409a39;
}

.confirm-input {
    margin-left: 1em;
}
</style>
