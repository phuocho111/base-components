<template>
  <div>
    <BaseBreadscrumbs :items="items" />
    <br />
    <BaseBreadscrumbs :items="items2" />
    <br />
    <BaseBreadscrumbs :items="items3" />
    <br />
    <BaseBreadscrumbs :items="items4" />
  </div>
</template>

<script>
import BaseBreadscrumbs from '@/components/commonApp/BaseBreadscrumbs.vue'
export default {
  components: {
    BaseBreadscrumbs,
  },
  layout: 'empty',
  data() {
    return {
      items: [{ text: 'Base Dialog', path: 'base-dialog' }],
      items2: [
        { text: 'Base Dialog', path: 'base-dialog' },
        { text: 'Base Table View', path: 'base-table-view' },
      ],
      items3: [
        { text: 'Base Form', path: 'base-form' },
        { text: 'Base Table View', path: 'base-table-view' },
        { text: 'Base Button', path: 'base-button' },
      ],
      items4: [
        { text: 'Base Dialog', path: 'base-dialog' },
        { text: 'Base Form', path: 'base-form' },
        { text: 'Base Table View', path: 'base-table-view' },
        { text: 'Base Button', path: 'base-button' },
        { text: 'Base Tree View', path: 'base-tree-view' },
        { text: 'Base Dragscroll', path: 'base-dragscroll' },
        { text: 'Base Input', path: 'base-input' },
        { text: 'Base Checkbox', path: 'base-checkbox' },
        { text: 'Base Pulldown', path: 'base-pulldown' },
        { text: 'Base Modal', path: 'base-modal' },
        { text: 'Base Icon', path: 'base-icon' },
        { text: 'Base Breadscrumbs', path: 'base-breadscrumbs' },
      ],
    }
  },
}
</script>
