<template>
  <div
    v-if="isLoading"
    :id="id"
    data-v-a0a529b0=""
    class="text-center bg-loading"
  >
    <div data-v-a0a529b0="" class="table-loading base-loading">
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isLoading: {
      type: Boolean,
      default: false,
    },
    id: {
      type: String,
      default: 'base-loading',
    },
  },
}
</script>
<style
  lang="scss"
  scoped
  src="@/assets/scss/components/commonApp/base-loading.scss"
></style>
