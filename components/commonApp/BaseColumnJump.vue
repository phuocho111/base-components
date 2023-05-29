<template>
  <div>
    <v-menu :close-on-content-click="false" :nudge-width="200" offset-x>
      <template #activator="{ on, attrs }">
        <div
          class="header-jump"
          :class="
            tableActionsJump.edit && tableActionsJump.delete
              ? 'header-jump-width'
              : ''
          "
          v-bind="attrs"
          v-on="on"
          @click="
            {
              handlePositionMenu()
            }
          "
        ></div>
      </template>
    </v-menu>
    <div id="menu-cus">
      <div v-if="isOpen" v-click-outside="hidePopover" class="menu">
        <v-list class="menu-popover">
          <v-list-item
            v-for="(item, headerId) in listHeaderViewport"
            :key="headerId"
            :class="[
              'menu-popover-content',
              item.visible && 'hidden-element',
              listHeaderViewport.every((item) => item.visible === false) &&
                'hidden-model',
            ]"
            @click="selectMenuItem($event, headerId)"
          >
            <span>{{ item.label }}</span>
          </v-list-item>
        </v-list>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    listHeader: {
      type: Array,
      default() {
        return []
      },
    },
    listColumnViewport: {
      type: Array,
      default() {
        return []
      },
    },
    tableActionsJump: {
      type: Object,
      default() {
        return {
          edit: false,
          delete: false,
        }
      },
    },
  },
  data() {
    return {
      isOpen: false,
      listHeaderViewport: [],
      top: 0,
    }
  },
  watch: {
    listColumnViewport(val) {
      this.listHeaderViewport = val
    },
  },
  methods: {
    handlePositionMenu() {
      const el = document.getElementsByClassName('header-jump')
      if (el?.length > 0) {
        const offsettop = el[0].getBoundingClientRect().top + 34
        this.top = offsettop
        const menu = document.getElementById('menu-cus')
        menu.style.cssText = `top: ${offsettop}px !important; right: 0; position: fixed !important; display: block !important;`
        this.isOpen = !this.isOpen
      }
    },
    selectMenuItem(event, headerId) {
      this.isOpen = false
      event.preventDefault()
      const header = document.getElementById(headerId)
      if (header) {
        header.scrollIntoView({
          behavior: 'smooth',
          block: 'nearest',
          inline: 'start',
        })
      }
    },
    hidePopover() {
      this.isOpen = false
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-column-jump.scss"
></style>
