<template>
  <div
    class="base-panel-error"
    :style="{ width, height }"
    :class="isOnModal && 'on-modal'"
  >
    <BaseIcon
      v-if="icon"
      :width="widthIcon"
      :height="heightIcon"
      :iconName="icon"
    />
    <div class="message">{{ message }}</div>
  </div>
</template>

<script>
import BaseIcon from './BaseIcon.vue'
export default {
  name: 'BasePanelError',
  components: {
    BaseIcon,
  },
  props: {
    message: {
      type: String,
      required: true,
      default: '登録データがありません。',
    },
    icon: {
      type: String,
      default: 'ico-panel-error',
    },
    width: {
      type: String,
      default: '100%',
    },
    height: {
      type: String,
      default: '100%',
    },
    widthIcon: {
      type: Number,
      default: 72,
    },
    heightIcon: {
      type: Number,
      default: 60,
    },
    isOnModal: {
      type: Boolean,
      default: false,
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-panel-error.scss"
></style>
