<template>
  <section>
    <h2 class="mb-2">ICON LIST</h2>
    <span class="ml-2"
      ><v-switch v-model="hover" label="change background" width="20"></v-switch
    ></span>
    <div class="list-icon" :class="{ hover: hover }">
      <div v-for="icon in [...icons].sort()" :key="`icon-${icon}`">
        <BaseIcon width="30" height="30" :iconName="icon" />
        <span class="ml-2">{{ icon }}</span>
      </div>
    </div>

    <br />
    <br />
    <hr />
    <h2 class="mb-2">SIZING</h2>
    <template v-for="size in sizes">
      <span :key="`size-${size}`">
        <BaseIcon :width="size" :height="size" iconName="ico-company" />
        <span class="ml-2 mr-5">{{ size }}</span>
      </span>
    </template>
    <br />
    <br />
    <hr />
    <h2 class="mb-2">FILTER COLOR</h2>
    <template v-for="filterColor in filterColors">
      <span :key="`filterColor-${filterColor}`">
        <BaseIcon iconName="ico-company" :filterColor="filterColor" />
        <span class="ml-2 mr-5">{{ filterColor }}</span>
      </span>
    </template>

    <br />
    <br />
    <hr />
    <h2 class="mb-2">HOVER FILTER COLOR</h2>
    <template v-for="filterColor in filterColors">
      <span :key="`filterColorHover-${filterColor}`">
        <BaseIcon iconName="ico-company" :hoverFilterColor="filterColor" />
        <span class="ml-2 mr-5">{{ filterColor }}</span>
      </span>
    </template>
    <br />
    <br />
    <hr />
    <h2 class="mb-2">TRANFORM ROTATE</h2>
    <template v-for="rotate in rotates">
      <span :key="`rotate-${rotate}`">
        <BaseIcon iconName="ico-company" :rotate="rotate" />
        <span class="ml-2 mr-5">{{ rotate }}</span>
      </span>
    </template>

    <br />
    <br />
    <hr />
    <h2>CURSOR TYPE</h2>
    <template v-for="cursor in cursors">
      <span :key="`cursor-${cursor}`">
        <BaseIcon iconName="ico-company" :cursorType="cursor" />
        <span class="ml-2 mr-5">{{ cursor }}</span>
      </span>
    </template>

    <br />
    <br />
  </section>
</template>

<script>
import BaseIcon from '@/components/commonApp/BaseIcon.vue'
export default {
  components: {
    BaseIcon,
  },
  layout: 'empty',
  data() {
    return {
      hover: true,
      filterColors: [
        'picton_blue_color',
        'lynch_color',
        'green_color',
        'night_shadz_color',
        'pickled_bluewood_color',
        'black',
      ],
      sizes: [10, 20, 30, 40, 50, 60, 70, 80, 90],
      rotates: [90, 180, 270],
      icons: [
        'ico-alert',
        'ico-angle',
        'ico-area',
        'ico-arrow-next',
        'ico-breadscrumb',
        'ico-buy-sell',
        'ico-calendar',
        'ico-check-mark',
        'ico-check',
        'ico-column-jump-disabled',
        'ico-column-jump-hover',
        'ico-column-jump',
        'ico-company',
        'ico-compas',
        'ico-contact',
        'ico-create-disabled',
        'ico-create',
        'ico-delete-auto-setting-hover',
        'ico-delete-auto-setting',
        'ico-edit-disabled',
        'ico-edit-hover',
        'ico-edit',
        'ico-error-disabled',
        'ico-error',
        'ico-export',
        'ico-factory',
        'ico-filter',
        'ico-filtered',
        'ico-graph-bar',
        'ico-help-hover',
        'ico-help',
        'ico-info',
        'ico-insurance',
        'ico-key',
        'ico-loader',
        'ico-maintenance',
        'ico-module',
        'ico-notification',
        'ico-number',
        'ico-open-new-tab',
        'ico-option-disabled',
        'ico-option-hover',
        'ico-option',
        'ico-password-hide',
        'ico-password',
        'ico-plant-contracted',
        'ico-plantbg',
        'ico-power-plan',
        'ico-rate-arrow',
        'ico-report-management',
        'ico-report',
        'ico-save-disabled',
        'ico-save',
        'ico-search',
        'ico-select',
        'ico-send-disabled',
        'ico-send',
        'ico-setting',
        'ico-sidebar',
        'ico-simulation',
        'ico-subsidy',
        'ico-supplier',
        'ico-time',
        'ico-timer-disabled',
        'ico-timer',
        'ico-trash',
        'ico-usage',
        'ico-user-auth',
        'ico-user',
      ],
      cursors: [
        'auto',
        'inherit',
        'crosshair',
        'default',
        'help',
        'move',
        'pointer',
        'progress',
        'text',
        'wait',
        'e-resize',
        'ne-resize',
        'nw-resize',
        'n-resize',
        'se-resize',
        'sw-resize',
        's-resize',
        'w-resize',
        'none',
        'context-menu',
        'cell',
        'vertical-text',
        'alias',
        'copy',
        'no-drop',
        'not-allowed',
        'ew-resize',
        'ns-resize',
        'nesw-resize',
        'nwse-resize',
        'col-resize',
        'row-resize',
        'all-scroll',
        'grab',
        'grabbing',
        'zoom-in',
        'zoom-out',
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.list-icon {
  margin: unset 10px;
  border-radius: 8px;
  padding: 10px;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
  &.hover {
    background-color: ghostwhite;
    color: red;
  }
}
</style>
