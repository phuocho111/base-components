<template>
  <div class="base-tab">
    <div class="tab-contents">
      <div class="tab-items">
        <div
          v-for="(item, i) in tabItems"
          :key="i"
          :value="item"
          @click="$emit('input', item.value)"
        >
          <div class="tab-item" :class="{ active: item.value == value }">
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    tabItems: {
      type: [Array],
      default: () => [],
    },
    value: {
      type: [String, Number],
      default: '',
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-tab.scss"
></style>
