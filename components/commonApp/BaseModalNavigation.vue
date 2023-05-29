<template>
  <div v-if="currentRecord > 0" class="base-navigation">
    <NewButton
      :class="[
        totalRecords === 0 || !val.id || val.id === 1 || disabled
          ? 'disable bg-disable'
          : 'default none-active',
      ]"
      :width="40"
      :disabled="disabled"
      @action="handlePrevious"
    >
      <BaseIcon
        iconName="ico-arrow-next"
        :rotate="270"
        :width="20"
        :height="20"
        :filter-color="getFilterPrevious"
      />
    </NewButton>
    <div>
      <BasePulldown
        v-model="val"
        class="common-select"
        :items="data"
        :item-text="getText"
        :is-disabled="disabled"
      />
    </div>
    <NewButton
      :class="[
        totalRecords === 0 || val.id === totalRecords || disabled
          ? 'disable bg-disable'
          : 'default none-active',
      ]"
      :disabled="disabled"
      :width="40"
      @action="handleNext"
    >
      <BaseIcon
        iconName="ico-arrow-next"
        :rotate="90"
        :width="20"
        :height="20"
        :filter-color="getFilterNext"
      />
    </NewButton>
    <BaseDialog
      :visible="confirmInfo.confirmDialog"
      :submitLabel="confirmInfo.submitLabel"
      :cancelLabel="confirmInfo.cancelLabel"
      :message="confirmInfo.confirmMessage"
      :variant="confirmInfo.variant"
      @submit="handleSubmit"
      @close="handleCancel"
    />
  </div>
</template>
<script>
import NewButton from '@/components/commonApp/NewButton.vue'
import BaseIcon from '@/components/commonApp/BaseIcon.vue'
import BasePulldown from '@/components/commonApp/BasePulldown.vue'
import { PAD_NUMBER, MIN_PAD_NUMBER } from '@/constants/modal'
import BaseDialog from '@/components/commonApp/BaseDialog.vue'

export default {
  name: 'BaseModalNavigation',
  components: {
    NewButton,
    BaseIcon,
    BasePulldown,
    BaseDialog,
  },
  props: {
    currentRecord: {
      type: Number,
      default: 0,
    },
    totalRecords: {
      type: Number,
      default: 0,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    val: {},
    padNumber: 4,
    data: [],
    iconFilterBluewood: 'pickled_bluewood_color',
    confirmInfo: {
      confirmDialog: false,
      confirmMessage:
        '操作を中止します。設定内容は保存されません。\nよろしいですか？',
      submitLabel: 'OK',
      cancelLabel: 'キャンセル',
      variant: 'warning',
    },
    oldVal: {},
  }),
  computed: {
    getFilterPrevious() {
      return this.totalRecords === 0 ||
        !this.val.id ||
        this.val.id === 1 ||
        this.disabled
        ? this.iconFilterBluewood
        : ''
    },
    getFilterNext() {
      return this.totalRecords === 0 ||
        this.val.id === this.totalRecords ||
        this.disabled
        ? this.iconFilterBluewood
        : ''
    },
  },
  watch: {
    val(newValue, oldValue) {
      if (!this.needConfirmOnLeave) {
        this.$emit('change', newValue)
      } else if (this.isEditedData) {
        if (newValue !== this.oldVal) {
          this.confirmInfo.confirmDialog = true
          this.oldVal = oldValue
        } else {
          this.oldVal = newValue
        }
      } else {
        this.oldVal = newValue
        this.$emit('change', newValue)
      }
    },
    currentRecord() {
      this.init()
    },
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      const padCurrent = (this.totalRecords + '').length
      if (padCurrent < MIN_PAD_NUMBER) {
        this.padNumber = MIN_PAD_NUMBER
      } else if (padCurrent > MIN_PAD_NUMBER) {
        this.padNumber = padCurrent + 1
      }
      this.val = {
        id: this.currentRecord,
        text: this.getPad(this.currentRecord, PAD_NUMBER.ZERO),
        value: this.getCurentRecord(this.currentRecord),
      }

      this.data = Array.from(Array(this.totalRecords).keys()).map(
        (item, index) => ({
          ...item,
          id: index + 1,
          text: this.getPad(index + 1, PAD_NUMBER.ZERO),
          value: this.getCurentRecord(index + 1),
        })
      )
    },
    getPad(value, prefix) {
      return (new Array(this.padNumber).join(prefix + '') + value).slice(
        -this.padNumber
      )
    },
    handleNext() {
      if (this.val.id === this.totalRecords) return
      const newValue = this.val.id + 1
      this.handleChangeValue(newValue)
    },
    handlePrevious() {
      if (!this.val.id || this.val.id === 1) return
      const newValue = this.val.id - 1
      this.handleChangeValue(newValue)
    },
    getCurentRecord(value) {
      return (
        this.getPad(value, PAD_NUMBER.ZERO) +
        '/' +
        this.getPad(this.totalRecords, PAD_NUMBER.ZERO)
      )
    },
    getText: (item) => item.value,
    handleChangeValue(event) {
      this.val = {
        id: event,
        text: this.getPad(event, PAD_NUMBER.ZERO),
        value: this.getCurentRecord(event),
      }
    },
    handleSubmit() {
      this.oldVal = this.val
      this.confirmInfo.confirmDialog = false
      this.$emit('change', this.val)
    },
    handleCancel() {
      this.confirmInfo.confirmDialog = false
      this.val = this.oldVal
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-modal-navigation.scss"
></style>
