<template>
  <div
    class="pagination-container"
    :class="[
      showSlot && 'has-slot',
      (alwaysShow || totalPage > 1) && 'always-show',
    ]"
  >
    <div v-if="totalPage > 1 || alwaysShow" class="base-pagination">
      <div
        class="base-pagination-container"
        :style="{ maxWidth: `${width ? width : '100%'}` }"
      >
        <NewButton
          :class="[
            !totalPage || !value || value === 1
              ? 'disable'
              : 'default none-active',
          ]"
          :width="40"
          @action="handlePrevious"
        >
          <BaseIcon
            iconName="ico-arrow-next"
            :rotate="180"
            :width="20"
            :height="20"
            :filter-color="getFilterPrevious"
          />
        </NewButton>
        <div class="pulldown">
          <BasePulldown
            v-model="pageSelected"
            class="common-select"
            :class="totalPage === 0 ? 'prevent-events-none' : ''"
            :items="listPage"
            @change="onChange"
          />
        </div>
        <NewButton
          :class="[
            value === totalPage || !totalPage
              ? 'disable'
              : 'default none-active',
          ]"
          :width="40"
          @action="handleNext"
        >
          <BaseIcon
            iconName="ico-arrow-next"
            :width="20"
            :height="20"
            :filter-color="getFilterNext"
          />
        </NewButton>
      </div>
      <div v-if="showSlot" class="ml-5 w-100 slot-container">
        <slot name="content"></slot>
      </div>
    </div>
  </div>
</template>

<script>
import BasePulldown from './BasePulldown'
import BaseIcon from './BaseIcon'
import NewButton from './NewButton'
export default {
  name: 'BasePagination',
  components: {
    BasePulldown,
    BaseIcon,
    NewButton,
  },
  props: {
    totalPage: {
      type: Number,
      required: true,
      default: 0,
    },
    value: {
      type: Number,
      required: true,
      default: 1,
    },
    width: {
      type: String,
      default: '',
    },
    alwaysShow: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    listPage: [],
    val: 0,
    iconFilterBluewood: 'pickled_bluewood_color',
    showSlot: false,
  }),
  computed: {
    pageSelected: {
      get() {
        return {
          value: this.value,
          text: `${this.value}/${this.totalPage}`,
        }
      },
      set(page) {
        this.$emit('input', page.value)
      },
    },
    getFilterPrevious() {
      return !this.totalPage || !this.val || this.val === 1
        ? this.iconFilterBluewood
        : ''
    },
    getFilterNext() {
      return !this.totalPage || this.val === this.totalPage
        ? this.iconFilterBluewood
        : ''
    },
  },
  watch: {
    value(val) {
      this.val = val
    },
    totalPage() {
      this.getListPage()
    },
  },
  beforeUpdate() {
    this.setShowSlots()
  },
  created() {
    this.setShowSlots()
  },
  mounted() {
    this.val = this.value
    this.getListPage()
  },
  methods: {
    getListPage() {
      this.listPage = []
      for (let index = 1; index <= this.totalPage; index++) {
        this.listPage.push({
          value: index,
          text: `${index}/${this.totalPage}`,
        })
      }
      if (!this.listPage.length) {
        this.listPage.push({
          value: 1,
          text: '1/1',
        })
      }
    },
    handlePrevious() {
      this.$emit('input', this.value - 1)
      this.onChange(this.value - 1)
    },
    handleNext() {
      this.$emit('input', this.value + 1)
      this.onChange(this.value + 1)
    },
    onChange(event) {
      window.scrollTo({ top: 0 })
      const tableDragscroll = document.getElementsByClassName('table-container')
      if (tableDragscroll && tableDragscroll.length > 0) {
        tableDragscroll[0].scrollTo({ top: 0 })
      }
      this.$emit('change', event.value)
    },
    setShowSlots() {
      this.showSlot = this.$slots.content?.[0]
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-pagination.scss"
></style>
