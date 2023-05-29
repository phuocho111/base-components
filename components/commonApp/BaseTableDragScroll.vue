<template>
  <div class="base-drag-scroll-table">
    <div
      ref="el"
      class="table-container"
      :style="{
        cursor: isDragging ? 'grabbing' : 'default',
      }"
      @scroll="handleScroll"
      @mousedown="onMouseDown"
      @mouseup="onMouseUp"
    >
      <table v-if="tableItems?.length > 0" class="table table-striped">
        <thead class="thead">
          <tr>
            <th
              v-for="(item, i) in tableHeaders"
              :id="i"
              :key="'thead' + i"
              v-observe-visibility="onVisibilityChange"
              class="th"
              :class="i == 0 && 'p-40'"
              cols="2"
              :width="item.width ? `${item.width}px` : 'fit-content'"
              :min-width="item.minWidth"
            >
              <div class="d-flex justify-space-between align-center">
                <span class="header-title">{{ item.label }}</span>
                <div>
                  <BaseIcon
                    :cursorType="getCursorType(item.value)"
                    :iconName="getIconName($route.query?.column, item.value)"
                    :width="12"
                    :height="12"
                    hover-filter-color="picton_blue_color"
                    @click="showFilterPopover($event, item)"
                  />
                </div>
              </div>
            </th>
            <th
              v-show="tableActions"
              :key="'thead-action'"
              v-observe-visibility="onVisibilityChange"
              :style="{ cursor: 'pointer' }"
              class="th"
              width="20"
            >
              <BaseColumnJump
                :list-header="tableHeaders"
                :list-column-viewport="visibleItems"
                :tableActionsJump="tableActions"
              ></BaseColumnJump>
            </th>
          </tr>
        </thead>
        <tbody class="tbody">
          <tr v-for="(n, i) in tableItems" :key="'tbody-tr' + i" class="tr">
            <td
              v-for="(k, j) in tableHeaders"
              :key="'tbody-tr-td' + j"
              class="td"
              :class="[`align-${k.align ? k.align : 'left'}`, j == 0 && 'p-40']"
              :width="n.width ? `${n.width}px` : 'fit-content'"
              :min-width="n.minWidth"
            >
              <template v-if="k.hasOwnProperty('cellRendererFramework')">
                <component
                  :is="k.cellRendererFramework"
                  v-if="k.cellRendererFramework === 'RendererButton'"
                  :value="n[k.value]"
                  @click="handleClickRender(n)"
                ></component>
              </template>
              <template v-else>
                {{ n[k.value] }}
              </template>
            </td>
            <td
              v-if="tableActions.edit || tableActions.delete"
              :key="'tbody-tr-td-action' + n"
              class="td nowrap action action-table"
              :style="{ cursor: 'pointer' }"
            >
              <div class="d-flex">
                <div
                  v-if="tableActions.edit"
                  class="action-edit-icon"
                  @click="editItem(n)"
                >
                  <BaseIcon
                    :width="16"
                    :height="16"
                    iconName="ico-edit"
                    cursorType="pointer"
                    hoverFilterColor="picton_blue_color"
                  />
                </div>
                <div
                  v-if="tableActions.delete"
                  class="action-more"
                  @click="showIconDelete($event, n)"
                >
                  <BaseIcon
                    :width="16"
                    :height="16"
                    iconName="ico-option"
                    cursorType="pointer"
                    hoverFilterColor="picton_blue_color"
                  />
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>

      <div
        v-show="showDeleteBar"
        :style="deleteContainerPosition"
        class="delete-container"
      >
        <div class="menu-list">
          <div class="list-item" @click="handleDelete()">
            <div class="item-icon">
              <BaseIcon
                :width="16"
                :height="16"
                iconName="ico-trash"
                cursorType="pointer"
                hoverFilterColor="night_shadz_color"
              />
            </div>
            <div class="item-title">削除</div>
          </div>
        </div>
      </div>
      <div
        ref="filterElement"
        :style="filterContainerPosition"
        class="filter-container"
      >
        <BaseFilterPopover
          :isShowFilter="showFilterBar"
          :columnName="currentItemFilter.value?.split('.').slice(-1).join('')"
          :isLoadingFilter="isLoadingFilter"
          :tableFilter="tableFilter"
          @handleFilter="handleFilter"
          @hanldeGetValueFilter="hanldeGetValueFilter(currentItemFilter.value)"
          @loadMore="
            ($event) => handleLoadmoreFilter(currentItemFilter.value, $event)
          "
          @searchFilter="
            ($event) => searchFilter(currentItemFilter.value, $event)
          "
          @changeProp="handleChangeProp"
        ></BaseFilterPopover>
      </div>
      <template
        v-if="(!tableItems || tableItems.length === 0) && !firstLoading"
      >
        <BasePanelError :message="messageNodata" />
      </template>
      <BaseLoading v-show="isLoading" id="table-loading" isLoading />
    </div>
    <BaseDialog
      :variant="variant ? 'warning' : 'info'"
      :message="
        !hasRelation
          ? messageActionMore?.title?.title_delete
          : messageActionMore?.title?.title_rel
      "
      :visible="dialog"
      :submitLabel="!hasRelation ? messageActionMore?.submit : 'OK'"
      :cancelLabel="!hasRelation ? messageActionMore?.cancel : null"
      @submit="!hasRelation ? delItem() : handleCancel()"
      @close="handleCancel"
    />
  </div>
</template>

<script>
import RendererButton from '../cell-renderer/RendererButton.vue'
import BaseFilterPopover from './BaseFilterPopover.vue'
import BaseIcon from './BaseIcon.vue'
import BaseColumnJump from './BaseColumnJump.vue'
import BasePanelError from './BasePanelError.vue'
import BaseLoading from './BaseLoading.vue'
import BaseDialog from '@/components/commonApp/BaseDialog.vue'
export default {
  components: {
    BaseFilterPopover,
    BaseIcon,
    BaseColumnJump,
    BaseDialog,
    BasePanelError,
    BaseLoading,
    RendererButton,
  },
  props: {
    tableHeaders: {
      type: Array,
      default() {
        return []
      },
    },
    tableItems: {
      type: Array,
      default() {
        return []
      },
    },
    tableActions: {
      type: Object,
      default() {
        return {
          edit: false,
          delete: false,
        }
      },
    },
    tableFilter: {
      type: Array,
      default: () => [],
    },
    messageActionMore: {
      type: Object,
      default() {
        return {
          title: {
            title_delete: '',
            title_rel: '',
          },
          submit: '',
          cancel: '',
        }
      },
    },
    hasRelation: {
      type: Boolean,
      default: false,
    },
    messageNodata: {
      type: String,
      default: '登録データがありません。',
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
    isLoadingFilter: {
      type: Boolean,
      default: false,
    },
    variant: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      isDragging: false,
      cursorPos: [0, 0],
      startScroll: [0, 0],
      el: null,
      visibleItems: [],
      dialog: false,
      deleteItem: null,
      firstLoading: true,
      isIcon: false,
      showDeleteBar: false,
      iconPositionDelete: { x: 0, y: 0 },
      iconPositionFilter: { x: 0, y: 0 },
      currentItemDelete: {},
      showFilterBar: false,
      currentItemFilter: {},
    }
  },
  computed: {
    deleteContainerPosition() {
      const top = `${this.iconPositionDelete.y}px`
      return { top, right: '0' }
    },
    filterContainerPosition() {
      const left = `${this.iconPositionFilter.x}px`
      return { top: '35px', left }
    },
    getCursorType() {
      return (itemValue) => {
        return itemValue !== 'index' ? 'pointer' : 'default'
      }
    },
    getIconName() {
      return (queryColumn, itemValue) => {
        const columnName =
          queryColumn === itemValue?.split('.').slice(-1).join('') ||
          (!queryColumn && itemValue?.split('.').slice(-1).join('') === 'index')
        return columnName ? 'ico-filtered' : 'ico-filter'
      }
    },
  },
  watch: {
    isLoading(val) {
      if (!val) this.firstLoading = false
    },
  },
  created() {
    window.addEventListener('resize', this.setHeightTabel)
  },
  mounted() {
    window.addEventListener('mouseup', this.onMouseUp)
    window.addEventListener('mousemove', this.onMouseHold)
    window.addEventListener('mousedown', this.onClickFilter)
    this.setHeightTabel()
  },
  beforeUpdate() {
    this.setHeightTabel()
  },
  destroyed() {
    window.removeEventListener('mouseup', this.onMouseUp)
    window.removeEventListener('resize', this.setHeightTabel)
    window.removeEventListener('mousemove', this.onMouseHold)
    window.removeEventListener('mousedown', this.onClickFilter)
  },
  methods: {
    setHeightTabel() {
      const header = document.getElementsByTagName('header')
      let headerHeight = 0
      if (header && header.length > 0) {
        if (window.innerWidth >= 1180) {
          headerHeight = header[header.length - 1].offsetHeight
        } else {
          headerHeight = header[0].offsetHeight
        }
      }

      const headerPage = document.getElementById('header-page')
      let headerPageHeight = 0
      if (headerPage) {
        headerPageHeight = headerPage.offsetHeight
      }

      const navigator = document.getElementsByClassName('navigator')
      let navigatorHeight = 0
      if (navigator && navigator.length > 0) {
        navigatorHeight = navigator[0].offsetHeight
      }

      const pagination = document.getElementsByClassName('pagination-container')
      let paginationHeight = 0
      if (pagination && pagination.length > 0) {
        paginationHeight = Array.from(pagination[0].classList).includes(
          'always-show'
        )
          ? pagination[0].offsetHeight
          : 0
      }

      const table = document.getElementsByClassName('table-container')
      if (table && table.length > 0) {
        table[0].style.height = `calc(100vh - ${headerHeight}px - ${headerPageHeight}px - ${paginationHeight}px - ${navigatorHeight}px)`
      }

      const tableDrag = document.getElementsByClassName(
        'base-drag-scroll-table'
      )
      if (tableDrag && tableDrag.length > 0) {
        tableDrag[0].style.height = `calc(100% - ${headerPageHeight}px - ${paginationHeight}px)`
      }

      const tableLoading = document.getElementById('table-loading')
      if (tableLoading) {
        tableLoading.style.height = `calc(100vh - ${headerHeight}px - ${headerPageHeight}px - ${paginationHeight}px)`
      }
    },
    onMouseDown(ev) {
      if (this.showDeleteBar) {
        setTimeout(() => {
          this.showDeleteBar = false
        }, 200)
      }
      this.cursorPos = [ev.pageX, ev.pageY]
      this.isDragging = true
    },
    onMouseUp(ev) {
      this.isDragging = false
    },
    onMouseHold(ev) {
      if (!this.isDragging) return false
      ev.preventDefault()
      if (!this.$refs.el) return
      requestAnimationFrame(() => {
        const delta = [
          ev.pageX - this.cursorPos[0],
          ev.pageY - this.cursorPos[1],
        ]

        if (Math.abs(delta[0]) > 10 || Math.abs(delta[1]) > 10) {
          this.cursorPos = [ev.pageX, ev.pageY]
          this.$refs.el.scrollBy({
            left: -delta[0],
            top: -delta[1],
          })
        }
      })
    },
    onVisibilityChange(isVisible, entry) {
      this.visibleItems = this.tableHeaders
      this.visibleItems.forEach((item, i) => {
        if (item.label === entry.target.innerText.trim()) {
          this.$set(this.visibleItems[i], 'visible', isVisible)
        }
      })
    },
    editItem(item) {
      this.$emit('edit-item', item)
    },
    handleDelete() {
      this.$emit('before-delete', this.currentItemDelete)
      this.deleteItem = this.currentItemDelete
      this.showDeleteBar = false
      this.dialog = true
    },
    delItem() {
      this.$emit('delete-item', this.deleteItem)
      this.dialog = false
    },
    handleCancel() {
      this.deleteItem = null
      this.dialog = false
    },
    hanldeGetValueFilter(value) {
      this.$emit('hanldeGetValueFilter', value)
    },
    handleFilter(value) {
      this.$emit('handleFilter', value)
    },
    handleLoadmoreFilter(column, value) {
      this.$emit('hanldeLoadmoreFilter', column, value)
    },
    handleClickRender(n) {
      this.$emit('click-render', n)
    },
    showIconDelete(event, item) {
      this.currentItemDelete = item
      const positionItemX = event?.x
      const positionItemY = event?.y
      const tablePostion = this.$refs.el?.getBoundingClientRect()
      this.showDeleteBar = true
      const viewportHeight = window.innerHeight
      const viewportWidth = window.innerWidth
      if (positionItemY + 50 + 34 > viewportHeight - 132 - 52) {
        this.iconPositionDelete = {
          x: positionItemX,
          y: positionItemY - 52 - 72 - 34 - 34 - 17,
        }
      } else {
        this.iconPositionDelete = {
          x: positionItemX,
          y: positionItemY - tablePostion.top - 10,
        }
      }
    },

    showFilterPopover(event, item) {
      if (item.value !== 'index') {
        this.currentItemFilter = item
        const positionItemX = event?.x
        const positionItemY = event?.y
        const tablePostion = this.$refs.el?.getBoundingClientRect()
        this.showFilterBar = true
        this.$emit('hanldeGetValueFilter', item.value)
        const viewportHeight = window.innerHeight
        const viewportWidth = window.innerWidth
        if (positionItemX + 320 + 12 > viewportWidth) {
          this.iconPositionFilter = {
            x:
              viewportWidth > 1179
                ? viewportWidth - 320 - 230 - 16
                : viewportWidth - 320 - 16,
            y: positionItemY,
          }
        } else {
          this.iconPositionFilter = {
            x: positionItemX - tablePostion.left,
            y: positionItemY - tablePostion.top,
          }
        }
      }
    },
    handleScroll() {
      this.showDeleteBar = false
      this.showFilterBar = false
    },
    searchFilter(column, value) {
      this.$emit('searchFilter', column, value)
    },
    handleChangeProp(newProp) {
      this.showFilterBar = newProp
    },
    onClickFilter(event) {
      if (!this.showFilterBar) return
      const pos = document.getElementsByClassName('filter-container')
      if (
        pos[0] !== event.target &&
        !event.target.closest('.filter-container')
      ) {
        this.showFilterBar = false
      }
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-drag-table.scss"
></style>
