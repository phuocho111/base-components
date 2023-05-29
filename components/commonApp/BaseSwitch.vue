<template>
  <div class="base-switch">
    <v-container fluid>
      <v-switch
        v-model="switchValue"
        :label="label"
        :value="true"
        :true-value="trueValue"
        :false-value="falseValue"
        :disabled="disabled"
        @blur="handleBlur()"
        @focus="handleFocus()"
        @change="handleChange($event)"
      ></v-switch>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'BaseSwitch',
  props: {
    value: {
      type: [String, Boolean],
      default: false,
    },
    label: {
      type: String,
      default: '',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    trueValue: {
      type: [String, Boolean],
      default: true,
    },
    falseValue: {
      type: [String, Boolean],
      default: false,
    },
  },
  computed: {
    switchValue: {
      get() {
        return this.value
      },
      set(newVal) {
        this.$emit('input', newVal)
      },
    },
  },
  methods: {
    handleBlur() {
      this.$emit('blur')
    },
    handleChange(event) {
      this.$emit('change', event)
    },
    handleFocus() {
      this.$emit('focus')
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-switch.scss"
></style>
