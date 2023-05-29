<template>
  <div class="base-select" :title="tooltipText">
    <v-select
      :ref="refId"
      v-model="selected"
      :items="items"
      :menu-props="{
        contentClass: `area-pulldown ${contentClass}`,
      }"
      :placeholder="placeholder"
      no-data-text="登録データがありません。"
      :error="error"
      class="pa-0 ma-0"
      :class="{ 'error-status': error, disabled: disabled }"
      :item-disabled="itemDisabled"
      :multiple="multiple"
      :disabled="isDisabled"
      :return-object="returnObject"
      :item-text="itemText"
      @blur="handleBlur()"
      @focus="handleFocus()"
      @change="handleChange($event)"
    >
      <template #prepend-item>
        <div
          v-if="showAddOption"
          class="add-item"
          @click.prevent="handleAddOption"
        >
          <div>＋ 新規追加</div>
        </div>
        <div
          v-if="showSelectAll && items.length"
          class="add-item"
          @click="handleSelectAllOption"
        >
          すべて
        </div>
      </template>
      <template v-if="showDeleteOptions" #item="{ item }">
        <div class="delete-item">
          <BaseIcon
            iconName="ico-trash"
            :width="20"
            :height="20"
            class="mr-3 icon-remove"
            @click.prevent.stop="handleDeleteOption(item)"
          />
          <div>{{ item.text }}</div>
        </div>
      </template>
      <template v-else #item="{ item }">
        <v-list-item-content>
          <v-list-item-title :title="item.text">{{
            item.text
          }}</v-list-item-title>
        </v-list-item-content>
      </template>
    </v-select>
    <div v-if="error" class="error-message">{{ errorMessage }}</div>
  </div>
</template>
<script>
import BaseIcon from './BaseIcon.vue'
import { debounce } from '~/utils/debounce'
export default {
  components: { BaseIcon },
  props: {
    value: {
      type: [Array, String, Object],
      default: () => [],
    },
    items: {
      type: Array,
      default: () => [],
    },
    error: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    errorMessage: {
      type: String,
      default: '',
    },
    returnObject: {
      type: Boolean,
      default: true,
    },
    itemDisabled: {
      type: String,
      default: 'disabled',
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    isDisabled: {
      type: Boolean,
      default: false,
    },
    showAddOption: {
      type: Boolean,
      default: false,
    },
    showSelectAll: {
      type: Boolean,
      default: false,
    },
    showDeleteOptions: {
      type: Boolean,
      default: false,
    },
    placeholder: {
      type: String,
      default: '',
    },
    itemText: {
      type: Function,
      default: (item) => item.text,
    },
    contentClass: {
      type: String,
      default: '',
    },
  },
  data: () => ({
    refId: 'basePulldown',
  }),

  computed: {
    selected: {
      get() {
        return this.value
      },
      set(sel) {
        this.$emit('input', sel)
      },
    },
    tooltipText() {
      if (this.returnObject) {
        if (Array.isArray(this.selected)) {
          return null
        } else {
          return this.selected?.text
        }
      } else if (Array.isArray(this.selected)) {
        return null
      } else {
        return this.items.find((item) => item.value === this.selected)?.text
      }
    },
  },
  beforeMount() {
    this.refId = 'basePulldown' + this._uid
  },
  mounted() {
    window.addEventListener('scroll', debounce(this.getPosition, 10))
    this.$refs[this.refId]?.$el?.addEventListener(
      'click',
      debounce(this.getPosition, 50)
    )

    window.addEventListener('resize', this.handleBlur)
  },
  destroyed() {
    window.removeEventListener('scroll', debounce(this.getPosition, 10))
    this.$refs[this.refId]?.$el?.removeEventListener(
      'click',
      debounce(this.getPosition, 50)
    )

    window.removeEventListener('resize', this.handleBlur)
  },
  methods: {
    getPosition() {
      const element = this.$refs[this.refId]
      if (
        element &&
        Array.from(element.$el.classList).includes('v-select--is-menu-active')
      ) {
        const rectPulldown = this.$refs[this.refId].$el?.getBoundingClientRect()

        const pullDown = document.getElementsByClassName(
          'menuable__content__active'
        )
        if (pullDown && pullDown.length > 0) {
          // check pulldown is open before
          if (element.$el.hasAttribute('isOpen')) {
            element.blur()
            return
          }
          const rectMenuItem = pullDown[0].getBoundingClientRect()
          const css = pullDown[0].style.cssText.split(';')
          // check menu item in viewport -> menu item is bottom
          if (window.innerHeight - rectPulldown.bottom >= rectMenuItem.height) {
            const changeCss = css.map((item) => {
              if (item.includes('top')) {
                item = `top: ${rectPulldown.top}px !important`
              }
              return item
            })

            pullDown[0].style.cssText = `${changeCss.join(
              ';'
            )}; position: fixed; transition: unset;`
          } else if (
            (rectMenuItem.top > 0 &&
              rectMenuItem.bottom > window.innerHeight) ||
            (rectMenuItem.top === 0 && rectMenuItem.bottom === 0) ||
            rectMenuItem.top < rectPulldown.top
          ) {
            if (rectPulldown.top === rectMenuItem.top + rectMenuItem.height)
              return

            const changeCss = css.map((item) => {
              if (item.includes('top')) {
                item = `top: ${
                  rectPulldown.top - rectMenuItem.height - 44
                }px !important`
              }
              return item
            })

            pullDown[0].style.cssText = `${changeCss.join(
              ';'
            )}; position: fixed; transition: unset;`
          }
          element.$el.setAttribute('isOpen', true)
        }
      }
    },

    handleBlur() {
      this.$refs[this.refId].$el.removeAttribute('isOpen')
      this.$emit('blur')
    },
    handleChange(event) {
      this.$refs[this.refId].$el.removeAttribute('isOpen')
      this.$emit('change', event)
    },
    handleFocus() {
      this.$emit('focus')
    },
    handleAddOption() {
      this.$emit('addOption')
    },
    handleDeleteOption(item) {
      this.$emit('deleteOption', item)
    },
    handleSelectAllOption() {
      this.$refs[this.refId].blur()
      this.$emit('selectAllOption')
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-pulldown.scss"
></style>
