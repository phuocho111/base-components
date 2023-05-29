<template>
  <v-btn
    v-click-outside="removeClass"
    :loading="isLoading"
    :width="width"
    :height="height"
    :disabled="disabled"
    :class="className"
    class="common-btn"
    @click.prevent="onBtnClick"
  >
    <slot />
  </v-btn>
</template>
<script>
export default {
  props: {
    width: {
      type: [String, Number],
      default: undefined,
    },
    height: {
      type: String,
      default: '40',
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      className: '',
    }
  },
  mounted() {
    if (navigator.userAgent.includes('Mac')) {
      this.className += 'mac-os'
    }
  },
  methods: {
    onBtnClick() {
      this.addClass()
      this.$emit('action')
    },
    addClass() {
      // remove bg after click in SP
      if (window.innerWidth < 1100) {
        this.className = 'clicked'
      }
    },
    removeClass() {
      this.className = ''
      if (navigator.userAgent.includes('Mac')) {
        this.className += 'mac-os'
      }
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-button.scss"
></style>
