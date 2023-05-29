<template>
  <div class="container" :class="containerClass">
    <div v-for="(option, index) in options" :key="index" class="grow">
      <input
        :id="name + option.value"
        v-model="selectOption"
        :name="name"
        type="radio"
        class="input-radio"
        :value="option.value"
      />
      <label :for="name + option.value">{{ option.label }}</label>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      type: [String, Boolean, Number],
      default: '',
    },
    name: {
      type: String,
      default: '1',
    },
    options: {
      type: Array,
      default: () => [{ label: '', value: '' }],
    },
    containerClass: {
      type: String,
      default: '',
    },
  },

  computed: {
    selectOption: {
      get() {
        return this.value
      },
      set(sel) {
        this.$emit('input', sel)
      },
    },
  },
  watch: {
    selectOption() {
      this.$emit('change', this.selectOption)
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-radio.scss"
></style>
