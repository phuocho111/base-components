<template>
  <v-dialog
    :key="key"
    ref="dialog"
    :value="visible"
    content-class="base-dialog"
    :persistent="true"
    :width="width"
  >
    <div class="content">
      <BaseIcon width="72" height="60" :iconName="iconName" />
      <div class="message">{{ message }}</div>
      <slot></slot>
    </div>
    <div v-if="submitLabel || cancelLabel" class="action">
      <div v-if="cancelLabel" class="w-100">
        <NewButton width="100%" class="default" @action="cancel">
          {{ cancelLabel }}
        </NewButton>
      </div>
      <div v-if="submitLabel" class="w-100">
        <NewButton width="100%" class="linear-gradient" @action="submit">
          {{ submitLabel }}
        </NewButton>
      </div>
    </div>
  </v-dialog>
</template>

<script>
export default {
  components: {
    NewButton: () => import('@/components/commonApp/NewButton.vue'),
    BaseIcon: () => import('@/components/commonApp/BaseIcon.vue'),
  },
  props: {
    visible: {
      type: Boolean,
      default: false,
    },
    width: {
      type: Number,
      default: 335,
    },
    variant: {
      type: String,
      default: 'info', // warning, info, loading
    },
    hideAction: {
      type: Boolean,
      default: false,
    },
    submitLabel: {
      type: String,
      default: '',
    },
    cancelLabel: {
      type: String,
      default: '',
    },
    message: {
      type: String,
      default: 'messsage',
    },
  },
  data() {
    return {
      key: 0,
      icons: [
        {
          variant: 'warning',
          iconName: 'ico-alert',
        },
        {
          variant: 'info',
          iconName: 'ico-info',
        },
        {
          variant: 'loading',
          iconName: 'ico-loader',
        },
      ],
    }
  },
  computed: {
    iconName() {
      return this.icons.find((icon) => icon.variant === this.variant)?.iconName
    },
  },
  watch: {
    visible(val) {
      if (val) {
        this.$refs.dialog.$el.blur()
      }
    },
  },
  methods: {
    cancel() {
      this.key++
      this.$emit('close')
    },
    submit() {
      this.key++
      this.$emit('submit')
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-dialog.scss"
></style>
