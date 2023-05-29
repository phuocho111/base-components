<template>
  <div class="cell-input-container" :class="{ 'input-error': error }">
    <input
      :value="value"
      :type="type"
      class="cell-input"
      :placeholder="placeholder"
      @input="onInput"
      @blur="onBlur"
      @keypress="onKeypress"
    />
  </div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'text', // text, number
    },
    value: {
      type: [Number, String],
      default: null,
    },
    error: {
      type: Boolean,
      default: false,
    },
    placeholder: {
      type: String,
      default: '',
    },
  },
  data() {
    return {}
  },
  methods: {
    onInput(event) {
      this.$emit('input', event.target.value)
    },
    onBlur() {
      this.$emit('blur')
    },
    onKeypress(event) {
      // const keyCode = event.keyCode
      // console.log("key", event.keyCode)
      // prevent if type === 'number, allow input key is number, negative number, dot
      // if (this.type === 'number') {
      //   if (keyCode < 45 || keyCode > 57 && event.keyCode || keyCode !== 45 || keyCode !== 46) {
      //     event.preventDefault()
      //   }
      // }
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-input-cell.scss"
></style>
