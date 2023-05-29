<template>
  <div class="base-input">
    <img
      v-if="iconSrc"
      class="input-icon__inner"
      :class="classList"
      :src="iconSrc"
      alt=""
      @click="handleClick"
    />
    <v-text-field
      :value="value"
      :type="typeInput"
      class="text-input"
      :class="{
        'input-error': error,
        'input-with-icon': iconSrc,
        'password-hide': !show,
      }"
      solo
      :placeholder="placeholder"
      :disabled="disabled"
      :suffix="subfixText"
      @input="$emit('input', $event)"
      @keyup.enter="onEnter"
      @click:append="handleClick"
    >
    </v-text-field>
    <p v-if="error && errMsg" class="error-message">{{ errMsg }}</p>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: '',
    },
    type: {
      type: String,
      default: 'text', // ['text', 'password', 'search']
    },
    placeholder: {
      type: String,
      default: '',
    },
    error: {
      type: Boolean,
      default: false,
    },
    errMsg: {
      type: String,
      default: '',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    subfixText: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      show: false,
    }
  },
  computed: {
    iconSrc() {
      let src = ''
      switch (this.type) {
        case 'password':
          src = this.show
            ? '/images/base-icon/ico-password.svg'
            : '/images/base-icon/ico-password-hide.svg'
          break
        case 'search':
          src = '/images/base-icon/ico-search.svg'
          break
        default:
          break
      }
      return src
    },
    typeInput() {
      let typeOfInput
      if (this.type !== 'password') {
        typeOfInput = 'text'
      } else if (this.show) {
        typeOfInput = 'text'
      } else {
        typeOfInput = 'password'
      }
      return typeOfInput
    },
    classList() {
      switch (this.type) {
        case 'search':
          return 'icon-search'
        case 'password':
          return this.show ? 'icon-password' : 'icon-password-hide'
        default:
          return null
      }
    },
  },
  methods: {
    handleClick() {
      if (this.type === 'password') {
        this.show = !this.show
      }
      this.$emit('onClickSubfix')
    },
    onEnter() {
      this.$emit('handleEnterKeyup')
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-input.scss"
></style>
