<!-- eslint-disable vue/no-unused-components -->
<template>
  <img
    class="base-icon"
    alt="icon"
    :src="`${filePath}/${iconName}.${iconType}`"
    :width="width"
    :height="height"
    :style="`cursor: ${cursorType}`"
    :class="[hoverFilter, tranform, filter]"
    :title="titleHover"
    @click.prevent="$emit('click', $event)"
  />
</template>

<script>
export default {
  props: {
    iconName: {
      type: String,
      default: 'IconBox',
    },
    filePath: {
      type: String,
      default: '/images/base-icon',
    },
    iconType: {
      type: String,
      default: 'svg',
    },
    width: {
      type: [Number, String],
      default: 30,
    },
    height: {
      type: [Number, String],
      default: 30,
    },
    filterColor: {
      type: String,
      default: null,
    },
    hoverFilterColor: {
      type: String,
      default: null,
    },
    rotate: {
      type: [Number, String],
      default: null,
    },
    cursorType: {
      type: String,
      default: 'default',
    },
    titleHover: {
      type: String,
      default: '',
    },
  },

  computed: {
    filter() {
      return this.filterColor ? `filter_${this.filterColor}` : null
    },
    tranform() {
      return this.rotate ? `rotate_${this.rotate}` : null
    },
    hoverFilter() {
      return this.hoverFilterColor
        ? `hover-filter_${this.hoverFilterColor} `
        : null
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-icon.scss"
></style>
