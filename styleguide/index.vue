<template>
  <div>
    <div
      v-for="item in items"
      :key="item.name"
      class="mt-3"
      style="
        border: 1px solid white;
        max-width: 500px;
        background-color: aliceblue;
      "
    >
      <nuxt-link :to="`/styleguide/${item.path}`">
        <v-list-item-title class="ms-4">
          <strong class="font-20">{{ item.name }}</strong>
        </v-list-item-title>
      </nuxt-link>
    </div>
  </div>
</template>
<script>
export default {
  layout: 'empty',
  data: () => ({
    items: [
      { name: 'API TEST', path: 'base-api' },
      { name: 'Base Dialog', path: 'base-dialog' },
      { name: 'Base Button', path: 'base-button' },
      { name: 'Base Input', path: 'base-input' },
      { name: 'Base Checkbox', path: 'base-checkbox' },
      { name: 'Base Pulldown', path: 'base-pulldown' },
      { name: 'Base Modal', path: 'base-modal' },
      { name: 'Base Icon', path: 'base-icon' },
      { name: 'Base Breadscrumbs', path: 'base-breadscrumbs' },
      { name: 'Base Pagination', path: 'base-pagination' },
      { name: 'Base Header Page', path: 'base-header-page' },
      { name: 'Base Switch', path: 'base-switch' },
      { name: 'Base Panel Eror', path: 'base-panel-error' },
      { name: 'Base Tree View', path: 'base-tree-view' },
      { name: 'Base Table View', path: 'base-table-view' },
      { name: 'Base Tab', path: 'base-tab' },
      { name: 'Base Card', path: 'base-card' },
    ],
    visibleItems: 30,
  }),
}
</script>
