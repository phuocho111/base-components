<template>
  <div class="base-checkbox">
    <div>
      <input
        v-model="val"
        class="checkbox-btn"
        type="checkbox"
        :disabled="disabled"
        :class="{ checked: value, disabled: disabled }"
      />
    </div>
    <slot name="label">
      <span class="checkbox-label">{{ label }}</span>
    </slot>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      type: Boolean,
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
  },
  data() {
    return {
      val: false,
    }
  },
  watch: {
    val(val) {
      if (this.disabled) return
      this.$emit('input', val)
    },
  },
  mounted() {
    this.val = this.value
  },
  methods: {},
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-checkbox.scss"
></style>
