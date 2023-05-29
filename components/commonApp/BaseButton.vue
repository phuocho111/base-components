<template>
  <v-btn
    v-click-outside="removeClass"
    :dark="dark"
    height="40"
    :loading="isLoading"
    :class="className"
    class="common-btn"
    :color="color"
    :disabled="disabled"
    @click.prevent="onBtnClick"
  >
    {{ text }}
  </v-btn>
</template>
<script>
export default {
  props: {
    color: {
      type: String,
      default: '#35d7cc',
    },
    text: {
      type: String,
      default: 'button',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
    dark: {
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
<style scoped lang="scss">
.common-btn {
  min-width: 110px !important;
  height: 40px !important;
  color: $black_color !important;
  font-weight: 300 !important;
  letter-spacing: 0.7px !important;
  &.upload-btn {
    width: 120px;
    min-width: 120px !important;
  }
  &.leftmenu-btn {
    width: 170px !important;
  }
  &.paging {
    width: 80px;
    min-width: 80px !important;
  }
  &.disabled {
    background: #172129 !important;
    pointer-events: none;
    background-color: #172129 !important;
    .v-btn__content {
      font-size: 13px;
      font-weight: bold;
      line-height: 24px;
      color: #2e455c !important;
    }
  }
  &:disabled {
    background-color: #172129 !important;
  }
  // &:focus {
  //   background-color: #1F2E3D !important;
  // }
  // &:active {
  //   background-color: #1f2e3d !important;
  // }
  &:visited {
    background-color: #1f2e3d !important;
  }
}
</style>
