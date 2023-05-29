<template>
  <v-card v-if="isShowFilter" class="card container-popover">
    <div class="search-bar">
      <BaseInput
        v-model="searchText"
        type="search"
        placeholder="検索"
        @onClickSubfix="handleClick"
      />
    </div>
    <div class="card-list" @scroll="onScroll">
      <v-list-item
        v-for="(item, index) in filterItems"
        :key="'container' + item.value + item.label + index + _uid"
        class="card-list-item"
        :class="[index === 0 && 'card-list-first-item']"
      >
        <v-list-item-action>
          <BaseCheckbox
            v-if="previousColumn === columnName"
            :key="'item' + item.value + item.label + index + _uid"
            :value="item.value"
            :label="!item.label ? '（なし）' : item.label"
            :disabled="item.disable"
            @input="handleCheckboxFilter(index, $event)"
          ></BaseCheckbox>
        </v-list-item-action>
      </v-list-item>
    </div>
    <v-card-actions class="card-action">
      <NewButton class="default btn-filter" @action="handleFilter"
        >適用</NewButton
      >
      <NewButton class="default btn-filter" @action="handleCancel"
        >キャンセル</NewButton
      >
      <NewButton class="default btn-filter" @action="handleReset"
        >クリア</NewButton
      >
    </v-card-actions>
    <BaseLoading v-show="isLoadingFilter" id="filter-loading" isLoading />
  </v-card>
</template>
<script>
import BaseCheckbox from './BaseCheckbox.vue'
import BaseLoading from './BaseLoading.vue'
import NewButton from './NewButton.vue'
import BaseInput from './BaseInput.vue'
import { debounce } from '~/utils/debounce'

export default {
  components: { BaseCheckbox, BaseLoading, NewButton, BaseInput },
  props: {
    tableFilter: {
      type: Array,
      default: () => [],
    },
    isLoadingFilter: {
      type: Boolean,
      default: false,
    },
    columnName: {
      type: String,
      default: '',
    },
    isShowFilter: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      lastScrollUpdate: 0,
      key: 0,
      filterItems: [],
      filterItemsClone: [],
      previousColumn: '',
      searchText: '',
      searchTextClone: '',
      cacheData: [],
      cacheDataClone: [],
      newPropShowFilter: false,
    }
  },
  computed: {
    getAll() {
      return this.cacheData.every((filterItem) => !filterItem.value)
    },
    queryColumn() {
      return this.$route.query?.column || ''
    },
    queryFilter() {
      return this.$route.query?.filter
        ? decodeURIComponent(this.$route.query.filter)
        : ''
    },
  },
  watch: {
    tableFilter: {
      handler(val) {
        if (!this.isShowFilter) return
        if (this.queryColumn && !this.queryFilter) {
          const path = window.location.pathname
          const query = { ...this.$route.query }
          delete query.column
          this.$router
            .replace({
              path,
              query,
            })
            .catch(() => {})
        }

        const filterTmp = JSON.parse(JSON.stringify([]))
        // previous column is empty => is first open filter || query column not equal current filter column
        if (val?.length > 0) {
          if (!this.queryColumn || this.queryColumn !== this.columnName) {
            if (!this.previousColumn) {
              // 1. first open filter
              this.cacheData = []
              this.filterItemsClone = []
              filterTmp.unshift({
                value: false,
                label: 'すべて表示',
                text: 'すべて表示',
                disable: false,
              })
              val.forEach((item) => {
                filterTmp.push({
                  value: false,
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            } else if (this.previousColumn === this.columnName) {
              // 2. previous column === columnName => reopen/append data current column
              filterTmp.push({
                value:
                  this.cacheData.length > 0 ? this.cacheData[0].value : false,
                label: 'すべて表示',
                text: 'すべて表示',
                disable: false,
              })
              val.forEach((item) => {
                // check duplicate items
                const cacheIndex = this.isDuplicateCache({
                  label: item.text,
                  text: item.value,
                })
                filterTmp.push({
                  value:
                    cacheIndex < 0
                      ? filterTmp[0].value
                      : this.cacheData[cacheIndex].value,
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            } else {
              // 3. previous column !== columnName => open new column
              this.scrollTop()
              this.cacheData = []
              this.filterItemsClone = []
              this.searchText = ''
              filterTmp.push({
                value: false,
                label: 'すべて表示',
                text: 'すべて表示',
                disable: false,
              })
              val.forEach((item) => {
                filterTmp.push({
                  value: false,
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            }
          } else {
            const filter = JSON.parse(JSON.stringify(this.queryFilter))
            // 1. previous column is empty => is first open filter
            if (!this.previousColumn) {
              this.filterItemsClone = []
              this.cacheData = []
              val.forEach((item) => {
                filterTmp.push({
                  value: filter
                    .split('|')
                    .includes(!item.value ? 'null' : item.value.toString()),
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            } else if (this.previousColumn !== this.columnName) {
              // 2. previous column !== columnName => open new column
              this.scrollTop()
              this.filterItemsClone = []
              this.cacheData = []
              val.forEach((item) => {
                filterTmp.push({
                  value: filter
                    .split('|')
                    .includes(!item.value ? 'null' : item.value.toString()),
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            } else {
              // 3. previous column === columnName
              val.forEach((item) => {
                const cacheIndex = this.isDuplicateCache({
                  label: item.text,
                  text: item.value,
                })
                filterTmp.push({
                  value:
                    cacheIndex < 0 ? false : this.cacheData[cacheIndex].value,
                  label: item.text,
                  text: item.value,
                  disable: false,
                })
              })
            }
            const isCheckAll = filterTmp.every((item) => item.value)
            filterTmp.unshift({
              value: isCheckAll,
              label: 'すべて表示',
              text: 'すべて表示',
              disable: false,
            })
          }
        }

        // data display
        this.filterItems = JSON.parse(JSON.stringify(filterTmp))
        this.filterItemsClone = [
          ...this.filterItemsClone,
          ...JSON.parse(JSON.stringify(filterTmp)),
        ]
        this.previousColumn = this.columnName
        this.searchTextClone = this.searchText

        // cache data
        filterTmp.forEach((item, index) => {
          const cacheIndex = this.isDuplicateCache(item)
          if (cacheIndex < 0) {
            this.cacheData.push(item)
          } else if (cacheIndex > 0) {
            this.cacheData[cacheIndex].value = item.value
          }
        })
        this.cacheData = [
          ...new Set(this.cacheData.map((o) => JSON.stringify(o))),
        ].map((s) => JSON.parse(s))
        this.cacheDataClone = JSON.parse(JSON.stringify(this.cacheData))
      },
      deep: true,
      immediate: true,
    },
    searchText: debounce(function (newVal) {
      if (this.isShowFilter) this.$emit('searchFilter', newVal)
    }, 200),
    isShowFilter(val) {
      if (!val) {
        this.cleanData()
        this.isFirstLoading = true
      }
    },
  },
  methods: {
    isDuplicateItem(data, item) {
      if (!data || !item) return false
      const result = data.find(
        (arrItem) => arrItem.label === item.text && arrItem.text === item.value
      )
      if (result) return true
      return false
    },
    isDuplicateCache(item) {
      let result = -1
      if (!this.cacheData || this.cacheData.length === 0 || !item) return -1
      this.cacheData.forEach((arrItem, index) => {
        if (arrItem.label === item.label && arrItem.text === item.text) {
          result = index
        }
      })
      return result
    },
    changeValue() {
      this.value = filterItems.value
    },
    hanldeGetValueFilter() {
      this.$emit('hanldeGetValueFilter')
    },
    handleFilter() {
      if (this.getAll) {
        this.$emit('handleFilter', [])
      } else {
        const filter = this.cacheData
          .filter((item, index) => item.value && index > 0)
          .map((item) => encodeURIComponent(item.text))
          .join('|')
        this.$emit('handleFilter', filter)
      }
      this.filterItemsClone = JSON.parse(JSON.stringify(this.filterItems))
      this.$emit('changeProp', (this.newPropShowFilter = false))
    },
    handleCancel() {
      this.$emit('changeProp', false)
      this.cleanData()
    },

    handleReset() {
      this.cleanData()
      if (this.searchText) {
        this.searchText = ''
      } else {
        this.$emit('searchFilter', this.searchText)
      }
    },

    cleanData() {
      this.cacheData = []
      this.cacheDataClone = []
      this.filterItems = []
      this.filterItemsClone = []
      this.searchText = ''
      this.searchTextClone = this.searchText
      this.previousColumn = ''
    },

    handleCheckboxFilter(index, value) {
      if (this.filterItems[index].label === 'すべて表示') {
        if (this.cacheData[0].value && !value) {
          for (let i = 0; i < this.cacheData.length; i++) {
            this.cacheData[i].value = false
          }
        }
        this.filterItems.forEach((item) => {
          if (!item.disable) {
            item.value = value
          }
          const cacheIndex = this.isDuplicateCache(item)
          this.cacheData[cacheIndex].value = value
        })
      } else {
        this.filterItems[index].value = value
        const cacheIndex = this.isDuplicateCache(this.filterItems[index])
        this.cacheData[cacheIndex].value = value
        if (this.filterItems.length > 1) {
          const arrTmp = this.filterItems.slice(1, this.filterItems.length)
          this.filterItems[0].value = arrTmp.every((item) => item.value)

          for (let i = 0; i < this.cacheData.length; i++) {
            const itemFilter = this.filterItems.find(
              (item) =>
                item.label === this.cacheData[i].label &&
                item.text === this.cacheData[i].text
            )
            if (itemFilter) {
              this.cacheData[i].value = itemFilter.value
            }
          }
          const arrTmpCache = this.cacheData.slice(1, this.cacheData.length)
          this.cacheData[0].value = arrTmpCache.every((item) => item.value)
        }
      }
    },
    scrollTop() {
      const cardList = document.getElementsByClassName('card-list')
      if (cardList && cardList.length > 0) {
        cardList[0].scrollTo({
          top: 0,
        })
      }
    },
    onScroll(e) {
      const scrollBar = e.target
      if (
        Math.ceil(scrollBar.scrollHeight - scrollBar.scrollTop) ===
        scrollBar.clientHeight
      ) {
        this.$emit('loadMore', this.searchText)
      }
    },
    handleClick() {
      if (this.searchText !== this.searchTextClone) {
        this.$emit('searchFilter', this.searchText)
      }
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-filter-popover.scss"
></style>
