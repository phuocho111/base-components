<template>
  <div id="header-page" class="header-page-container">
    <div
      class="header-page-top"
      :class="[
        topClassName,
        isWrapMobile && 'wrapper-header-sp',
        !isShowMobile && 'd-none-sp',
      ]"
      :style="`grid-template-columns: ${topTemplateColumn}`"
    >
      <div class="header-page-left font-18">
        {{ title }}
      </div>
      <slot name="top"></slot>
    </div>
    <div class="header-page-bottom">
      <slot name="bottom"></slot>
    </div>
  </div>
</template>
<script>
export default {
  name: 'BaseHeaderPage',
  props: {
    title: {
      type: String,
      required: true,
      default: '',
    },
    topClassName: {
      type: String,
      default: '',
    },
    topTemplateColumn: {
      type: String,
      default: 'repeat(auto, 1fr)',
    },
    isWrapMobile: {
      type: Boolean,
      default: false,
    },
    isShowMobile: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({}),
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-header-page.scss"
></style>
