<template>
  <div class="base-card" @click="$emit('click', $event)">
    <div class="card-header">
      <div class="card-period">
        <div class="period" :class="statusVariant">{{ period }}</div>
        <BaseIcon
          v-if="isSubmited"
          iconName="ico-check-mark"
          width="18"
          height="18"
        />
      </div>
      <div class="card-item-name">{{ itemName }}</div>
    </div>
    <div class="card-body">
      <div class="date">{{ formatDate }}</div>
      <div class="card-action">
        <div v-if="showRemoveAuto" class="automatic-btn">
          <div class="automatic-text">自動</div>
          <BaseIcon
            :iconName="autoIcon"
            width="12px"
            height="12px"
            @mousemove.native="showHover"
            @mouseleave.native="removeHover"
            @click="handleRemoveAuto"
          />
        </div>
        <div v-if="showIntraday" class="today-btn">
          <div class="today-text">当日</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import BaseIcon from './BaseIcon.vue'
import { DateTimeConverter } from '@/utils/dateTimeConvert'
export default {
  components: { BaseIcon },
  props: {
    period: {
      type: String,
      default: '',
    },
    date: {
      type: String,
      default: '',
    },
    statusVariant: {
      type: String,
      default: '',
    },
    itemName: {
      type: String,
      default: '',
    },
    showRemoveAuto: {
      type: Boolean,
      default: false,
    },
    showIntraday: {
      type: Boolean,
      default: false,
    },
    isSubmited: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      autoIcon: 'ico-delete-auto-setting',
    }
  },
  computed: {
    formatDate() {
      return this.date ? DateTimeConverter.getDateOfPlan(this.date) : ''
    },
  },
  methods: {
    handleRemoveAuto(event) {
      this.$emit('action-automatic', event)
    },
    showHover() {
      this.autoIcon = 'ico-delete-auto-setting-hover'
    },
    removeHover() {
      this.autoIcon = 'ico-delete-auto-setting'
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-card.scss"
></style>
