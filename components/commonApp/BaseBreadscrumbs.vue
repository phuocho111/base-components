<template>
  <div class="base-breadscrumbs">
    <template v-for="(item, index) in items">
      <nuxt-link
        :key="`link_${item.text}`"
        :to="item.path"
        class="breadscrumb-item"
        >{{ item.text }}</nuxt-link
      >
      <template v-if="items.length > 1 && items.length - 2 >= index">
        <BaseIcon
          :key="`icon_${item.text}`"
          iconName="ico-breadscrumb"
          width="6"
          height="12"
          class="breadscrumb-divider"
        />
      </template>
    </template>
  </div>
</template>

<script>
import BaseIcon from '@/components/commonApp/BaseIcon.vue'
export default {
  components: {
    BaseIcon,
  },
  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },
}
</script>

<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-breadscrumbs.scss"
></style>
