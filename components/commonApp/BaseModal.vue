<template>
  <v-dialog
    ref="modal"
    v-model="show"
    content-class="base-modal"
    :persistent="persistent"
    :width="width"
    @click:outside="closeDialog()"
  >
    <template>
      <div class="modal-header">
        <div class="modal-header-container">
          <h1 class="modal-header-title">{{ title }}</h1>
          <BaseModalNavigation
            :current-record="currentRecord"
            :total-records="totalRecords"
            :disabled="disabledPagination"
            :needConfirmOnLeave="needConfirmOnLeave"
            :isEditedData="isEditedData"
            @change="getItem($event)"
          />
        </div>
        <div class="modal-header-info">
          <div
            v-if="accountName"
            class="modal-header-info_content modal-header-info_name"
          >
            最終更新者：{{ accountName }}
          </div>
          <div
            v-if="getDate"
            class="modal-header-info_content modal-header-info_date"
          >
            {{ getDate }}
          </div>
        </div>
      </div>
    </template>
    <template>
      <div
        class="modal-body"
        :class="!isHiddenScrollBody && 'modal-body-scroll'"
      >
        <slot></slot>
        <BaseLoading
          v-if="hasLoading"
          id="modal-loading"
          :isLoading="isLoading"
        />
      </div>
    </template>
    <template>
      <div class="modal-footer">
        <div class="modal-footer-container">
          <div v-if="!hideDeleteAction" class="modal-action">
            <NewButton
              class="modal-action-item"
              :class="disabledDeleteAction ? 'disable' : 'default '"
              @action="
                {
                  if (disabledDeleteAction) return false
                  handleAction(actionType.DELETE)
                }
              "
              >{{ labelDeleteAction }}</NewButton
            >
          </div>
          <div class="modal-action">
            <NewButton
              class="modal-action-item"
              :class="disabledCancelAction ? 'disable' : 'default'"
              @action="
                {
                  if (disabledCancelAction) return false
                  handleAction(actionType.CANCEL)
                }
              "
              >{{ labelCancelAction }}</NewButton
            >
            <NewButton
              class="modal-action-item"
              :class="disabledSubmitAction ? 'disable' : 'linear-gradient'"
              @action="
                {
                  if (disabledSubmitAction) return false
                  handleAction(actionType.SUBMIT)
                }
              "
              ><BaseIcon
                iconName="ico-save"
                :width="20"
                :height="20"
                :filter-color="getFilterIconSubmit"
              />{{ labelSubmitAction }}</NewButton
            >
          </div>
        </div>
      </div>
    </template>
  </v-dialog>
</template>

<script>
import BaseLoading from './BaseLoading.vue'
import BaseIcon from './BaseIcon.vue'
import BaseModalNavigation from './BaseModalNavigation.vue'
import NewButton from './NewButton.vue'
import { DateTimeConverter } from '~/utils/dateTimeConvert'
import { ACTION_TYPE } from '~/constants/modal'
import { FORMART_TYPE } from '~/constants/date'

export default {
  name: 'BaseModal',
  components: {
    BaseModalNavigation,
    NewButton,
    BaseIcon,
    BaseLoading,
  },
  props: {
    visible: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: '',
    },
    currentRecord: {
      type: Number,
      default: 0,
    },
    totalRecords: {
      type: Number,
      default: 0,
    },
    accountName: {
      type: String,
      default: '',
    },
    date: {
      type: [String, Date],
      default: '',
    },
    hideDeleteAction: {
      type: Boolean,
      default: false,
    },
    hideCancelAction: {
      type: Boolean,
      default: false,
    },
    hideSubmitAction: {
      type: Boolean,
      default: false,
    },
    disabledDeleteAction: {
      type: Boolean,
      default: false,
    },
    disabledCancelAction: {
      type: Boolean,
      default: false,
    },
    disabledSubmitAction: {
      type: Boolean,
      default: false,
    },
    labelDeleteAction: {
      type: String,
      default: '削除',
    },
    labelCancelAction: {
      type: String,
      default: 'キャンセル',
    },
    labelSubmitAction: {
      type: String,
      default: '保存',
    },
    width: {
      type: Number,
      default: 820,
    },
    persistent: {
      type: Boolean,
      default: false,
    },
    disabledPagination: {
      type: Boolean,
      default: false,
    },
    isHiddenScrollBody: {
      type: Boolean,
      default: false,
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
    hasLoading: {
      type: Boolean,
      default: false,
    },
    needConfirmOnLeave: {
      type: Boolean,
      default: false,
    },
    isEditedData: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    actionType: ACTION_TYPE,
  }),
  computed: {
    getDate() {
      if (!this.date) return ''
      return DateTimeConverter.convertAPIDateTimeToDisplay(
        new Date(this.date),
        FORMART_TYPE.ISO_STANDARD_AND_TIME
      )
    },
    getFilterIconSubmit() {
      return this.disabledSubmitAction ? 'pickled_bluewood_color' : ''
    },
    show: {
      get() {
        return this.visible
      },
      set(value) {
        if (!value) {
          this.$emit('close')
        }
      },
    },
  },
  methods: {
    handleAction(actionType) {
      this.$emit('click', actionType)
    },
    closeDialog() {
      this.$emit('click', ACTION_TYPE.CLICK_OUTSIDE)
    },
    getItem(event) {
      this.$emit('change', event.text)
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-modal.scss"
></style>
