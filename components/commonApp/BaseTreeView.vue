<template>
  <div class="tree-table-container">
    <div
      ref="el"
      class="tree-table"
      :style="{
        cursor: isDragging ? 'grabbing' : 'default',
      }"
      @mousedown="onMouseDown"
      @mouseup="onMouseUp"
      @touchstart="onTouchStart"
      @touchmove="onTouchMove"
      @touchend="onTouchEnd"
    >
      <div class="flex">
        <div
          v-for="(col, colIdx) in depth"
          :key="colIdx"
          class="tree-col-container"
          :class="[getTreeClassPadding(colIdx)]"
        >
          <div class="tree-header">
            <div class="flex justify-between align-center">
              <div v-if="hasCheckBox" class="tree-checkbox">
                <BaseCheckbox
                  :key="depthCheckbox[colIdx]"
                  :value="depthCheckbox[colIdx]"
                  @input="handleCheckCol($event, colIdx)"
                />
              </div>
              <div class="tree-ic-sort">
                <BaseFilterPopover></BaseFilterPopover>
              </div>
            </div>
          </div>
          <template v-for="(row, rowIdx) in dataTable">
            <div
              v-if="row.every((item) => item.index > -1)"
              :key="rowIdx"
              :class="['tree-row']"
            >
              <template v-if="colIdx === depth - 1 && row.length < depth">
                <div :class="['tree-col']"></div>
              </template>
              <template v-else-if="isHasData(rowIdx, colIdx)">
                <div
                  :id="`checkbox${dataTable[rowIdx][colIdx].id}`"
                  :class="[addClassNameColData(rowIdx, colIdx)]"
                  :data-index="
                    dataTable[rowIdx].length > colIdx
                      ? dataTable[rowIdx][colIdx].index
                      : null
                  "
                  :data-row-index="rowIdx"
                  :data-col-index="colIdx"
                >
                  <div v-if="hasCheckBox" class="tree-checkbox">
                    <BaseCheckbox
                      :key="
                        dataTable[rowIdx][colIdx].id +
                        '' +
                        dataTable[rowIdx][colIdx].isChecked
                      "
                      :value="dataTable[rowIdx][colIdx].isChecked"
                      :disabled="dataTable[rowIdx][colIdx].isDisabled"
                      @input="handleCheckbox(rowIdx, colIdx, $event)"
                    />
                  </div>
                  <div :class="['tree-col']">
                    <span class="tree-col-label">{{
                      dataTable[rowIdx][colIdx].label
                    }}</span>
                  </div>
                  <span
                    :class="
                      dataTable[rowIdx].length > colIdx &&
                      dataTable[rowIdx][colIdx].hasChildren
                        ? 'tree-col-data_line-right'
                        : ''
                    "
                  ></span>
                </div>
              </template>
              <template
                v-else-if="
                  dataTable[rowIdx].length > colIdx &&
                  dataTable[rowIdx][colIdx].index > -1
                "
              >
                <div
                  :class="[addClassNameColNoData(rowIdx, colIdx)]"
                  :data-index="
                    dataTable[rowIdx].length > colIdx
                      ? dataTable[rowIdx][colIdx].index
                      : null
                  "
                  :data-row-index="rowIdx"
                  :data-col-index="colIdx"
                ></div>
              </template>
            </div>
          </template>
        </div>
      </div>
    </div>
    <div v-show="isLoadingTable" class="bg-tree-table">
      <BaseLoading :isLoading="true" />
    </div>
  </div>
</template>

<script>
import BaseCheckbox from './BaseCheckbox.vue'
import BaseFilterPopover from './BaseFilterPopover.vue'
import BaseLoading from './BaseLoading.vue'
import { NestedObjectConvert } from '@/utils/nestedObjectConvert'

export default {
  name: 'BaseTreeView',
  components: {
    BaseCheckbox,
    BaseLoading,
    BaseFilterPopover,
  },
  props: {
    dataTree: {
      type: Array,
      default: () => [],
    },
    hasCheckBox: {
      type: Boolean,
      default: false,
    },
    isShowHistory: {
      type: Boolean,
      default: false,
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
    scrollToCheckboxId: {
      type: [String, Number],
      default: null,
    },
    idTable: {
      type: String,
      default: 'tree-table',
    },
  },
  data: () => ({
    depth: 0,
    currentTreeData: [],
    dataTable: [],
    depthCheckbox: [],
    classTable: {
      colClass: 'tree-col',
      noData: 'no-data',
      hideLine: 'hide-line',
    },
    isLoadingTable: false,
    isDragging: false,
    cursorPos: [0, 0],
    startScroll: [0, 0],
    el: null,
  }),
  computed: {},
  watch: {
    currentTreeData: {
      handler: function (newVal) {
        this.$emit('change', newVal)
      },
      deep: true,
    },
    isShowHistory() {
      this.changeLoading(true)
      this.drawTree()
      this.changeLoading(false)
    },
    dataTree() {
      this.changeLoading(true)
      this.refactorData()
      this.changeLoading(false)
    },
    scrollToCheckboxId() {
      setTimeout(() => this.scrollToItem(), 100)
    },
  },
  mounted() {
    window.addEventListener('mouseup', this.onMouseUp)
    this.scrollToItem()
  },
  destroyed() {
    window.removeEventListener('mouseup', this.onMouseUp)
  },
  beforeMount() {
    this.changeLoading(true)
    this.refactorData()
    this.changeLoading(false)
  },
  methods: {
    refactorData() {
      if (!this.dataTree) {
        this.depth = 1
      } else {
        this.depth = Math.max(
          ...this.dataTree.map((item) => NestedObjectConvert.getDepth(item))
        )
      }
      for (let i = 0; i < this.depth; i++) {
        this.depthCheckbox.push(false)
      }
      this.currentTreeData = JSON.parse(JSON.stringify(this.dataTree))
      this.drawTree()
    },

    drawTree() {
      this.dataTable = JSON.parse(
        JSON.stringify(
          NestedObjectConvert.getCombinations(
            { children: [...this.currentTreeData] },
            0,
            this.isShowHistory
          )
        )
      )
      for (let i = 0; i < this.depth; i++) {
        let isCheckedAllCol = true
        for (let j = 0; j < this.dataTable.length; j++) {
          if (this.dataTable[j].length > i) {
            if (!this.dataTable[j][i].isChecked) {
              isCheckedAllCol = false
            }
          }
        }
        this.depthCheckbox.splice(i, 1, isCheckedAllCol)
      }
    },

    handleCheckCol(value, colIdx) {
      this.changeLoading(true)
      try {
        this.depthCheckbox.splice(colIdx, 1, value)
        if (value) {
          this.handleCheckedByDepth(this.currentTreeData, value, colIdx, 0)
        } else {
          this.handleUnCheckedByDepth(this.currentTreeData, value, colIdx, 0)
          this.handleUnCheckedParentByDepth(this.currentTreeData, colIdx, 0)
        }
        this.drawTree()
      } finally {
        this.changeLoading(false)
      }
    },

    handleCheckedByDepth(data, value, depth, currentDepth) {
      if (currentDepth < depth) {
        for (const node of data) {
          if (node.children?.length > 0) {
            this.handleCheckedByDepth(
              node.children,
              value,
              depth,
              currentDepth + 1
            )
          }
        }
      } else if (currentDepth === depth) {
        for (const node of data) {
          if (!node.isDisabled) {
            node.isChecked = value
          }
        }
      }
    },

    handleUnCheckedByDepth(data, value, depth, currentDepth) {
      if (currentDepth < depth) {
        for (const node of data) {
          if (node.children?.length > 0) {
            this.handleUnCheckedByDepth(
              node.children,
              value,
              depth,
              currentDepth + 1
            )
          }
        }
      } else if (currentDepth === depth) {
        for (const node of data) {
          node.isChecked = node.isDisabled ? node.isChecked : value
          if (node.children?.length > 0 && !node.isChecked) {
            this.handleUnCheckedByDepth(
              node.children,
              value,
              depth,
              currentDepth + 1
            )
          }
        }
      } else if (currentDepth > depth && currentDepth !== this.depth) {
        for (const node of data) {
          if (!node.isDisabled) {
            node.isChecked = value
            this.handleUnCheckedByDepth(
              node.children,
              value,
              depth,
              currentDepth + 1
            )
          }
        }
      } else {
        for (const node of data) {
          if (!node.isDisabled) {
            node.isChecked = value
          }
        }
      }
    },

    handleUnCheckedParentByDepth(data, depth, currentDepth) {
      if (currentDepth + 1 < depth) {
        for (const node of data) {
          if (node.children?.length > 0) {
            this.handleUnCheckedParentByDepth(
              node.children,
              depth,
              currentDepth + 1
            )
          }
        }
      } else if (currentDepth + 1 === depth) {
        for (const node of data) {
          if (!node.isDisabled) {
            if (
              node.children.length > 0 &&
              node.children.some((child) => !child.isChecked)
            ) {
              node.isChecked = false
              this.handleUnCheckedParentByDepth(
                this.currentTreeData,
                currentDepth--,
                0
              )
            }
          }
        }
      } else if (currentDepth === 0 && currentDepth === depth) {
        for (const node of data) {
          if (!node.isDisabled) {
            if (
              node.children.length > 0 &&
              node.children.some((child) => !child.isChecked)
            ) {
              node.isChecked = false
            }
          }
        }
      }
    },

    handleCheckbox(rowIdx, colIdx, value) {
      this.recursiveChildNode(
        this.currentTreeData,
        this.dataTable[rowIdx][colIdx].id,
        value
      )
      this.recursiveParentNode(
        this.currentTreeData,
        this.dataTable[rowIdx][colIdx].id
      )
      this.drawTree()
    },

    recursiveChildNode(data, nodeId, value) {
      for (const node of data) {
        if (node.id === nodeId) {
          node.isChecked = !node.isDisabled ? value : node.isChecked
          if (node.children?.length > 0) {
            node.children.map((item) =>
              this.recursiveChildNode(node.children, item.id, value)
            )
          }
        } else if (node.children?.length > 0) {
          node.children.map((item) =>
            this.recursiveChildNode(node.children, nodeId, value)
          )
        }
      }
    },

    recursiveParentNode(data, nodeId) {
      for (const node of data) {
        if (node.children?.length > 0) {
          if (node.children.some((child) => child.id === nodeId)) {
            node.isChecked =
              node.children.every((child) => child.isChecked) &&
              !node.isDisabled
            this.recursiveParentNode(this.currentTreeData, node.id)
          } else {
            this.recursiveParentNode(node.children, nodeId)
          }
        }
      }
    },

    changeLoading(isOpen) {
      if (isOpen) {
        this.isLoadingTable = isOpen
      } else {
        setTimeout(() => {
          this.isLoadingTable = isOpen
        }, 1000)
      }
    },

    isHasData(rowIdx, colIdx) {
      return (
        (this.dataTable[rowIdx].length > colIdx &&
          this.dataTable[rowIdx][colIdx].index === 0) ||
        (this.dataTable[rowIdx].length > colIdx &&
          !this.dataTable[rowIdx][colIdx].hasChildren &&
          this.dataTable[rowIdx][colIdx].index >= 0)
      )
    },

    addClassNameColData(rowIdx, colIdx) {
      let className = `flex justify-start`
      if (colIdx > 0) className += ` ${this.classTable.colClass}-data`
      if (this.dataTable[rowIdx].length > colIdx && colIdx > 0) {
        if (
          this.dataTable[rowIdx][colIdx - 1].index === 0 &&
          this.dataTable[rowIdx][colIdx].index === 0 &&
          this.dataTable[rowIdx][colIdx - 1].hasChildren
        ) {
          className += ` ${this.classTable.colClass}-data_first`
        } else if (
          this.dataTable[rowIdx][colIdx - 1].index > 0 &&
          this.dataTable[rowIdx][colIdx - 1].hasChildren
        ) {
          className += ` ${this.classTable.colClass}-data_between`
        }
        const { length, isLast } = NestedObjectConvert.isLastChildOfNode(
          this.dataTree,
          this.dataTable[rowIdx][colIdx - 1].id,
          this.dataTable[rowIdx][colIdx].id,
          this.isShowHistory
        )
        if (isLast) {
          className += ` ${this.classTable.colClass}-data_last`
          if (length === 1) {
            className += ` ${this.classTable.colClass}-data_first-last`
          }
        }
      }
      return className
    },

    addClassNameColNoData(rowIdx, colIdx) {
      let className = ''
      if (colIdx > 0 && colIdx < this.dataTable[rowIdx].length) {
        className = `${this.classTable.colClass} ${this.classTable.colClass}-${this.classTable.noData}`
        const { length, isLast } = NestedObjectConvert.isLastChildOfNode(
          this.dataTree,
          this.dataTable[rowIdx][colIdx - 1].id,
          this.dataTable[rowIdx][colIdx].id
        )
        if (isLast && this.dataTable[rowIdx][colIdx].index > 0) {
          className += ` ${this.classTable.colClass}-no-data_last`
        }
      }
      return className
    },

    getTreeClassPadding(col) {
      if (!col) return 'tree-col-padding-left'
      if (col === this.depth - 1) return 'tree-col-padding-right'
      return ''
    },

    onMouseDown(ev) {
      this.cursorPos = [ev.pageX, ev.pageY]
      this.isDragging = true
      window.addEventListener('mousemove', this.onMouseHold)
    },

    onMouseUp(ev) {
      window.removeEventListener('mousemove', this.onMouseHold)
      this.isDragging = false
    },

    onMouseHold(ev) {
      ev.preventDefault()
      if (!this.$refs.el) return
      requestAnimationFrame(() => {
        const delta = [
          ev.pageX - this.cursorPos[0],
          ev.pageY - this.cursorPos[1],
        ]

        this.cursorPos = [ev.pageX, ev.pageY]

        this.$refs.el.scrollBy({
          left: -delta[0],
          top: -delta[1],
        })
      })
    },

    onTouchStart(event) {
      this.cursorPos = [event.touches[0].clientX, event.touches[0].clientY]
      this.startScroll = [this.$refs.el.scrollTop, this.$refs.el.scrollTop]
      this.isDragging = true
    },

    onTouchMove(event) {
      if (!this.isDragging) return
      event.preventDefault()
      const currentY = event.touches[0].clientY
      const currentX = event.touches[0].clientX
      const deltaX = currentX - this.cursorPos[0]
      const deltaY = currentY - this.cursorPos[1]

      this.cursorPos = [event.touches[0].clientX, event.touches[0].clientY]

      this.$refs.el.scrollBy({
        left: -deltaX,
        top: -deltaY,
      })
    },

    onTouchEnd(event) {
      this.isDragging = false
    },
    scrollToItem() {
      if (this.scrollToCheckboxId) {
        const item = document.getElementById(
          `checkbox${this.scrollToCheckboxId}`
        )
        if (item) {
          item.scrollIntoView({ behavior: 'smooth' })
        }
      }
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-tree-view.scss"
></style>
