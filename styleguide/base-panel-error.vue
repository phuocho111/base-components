<template>
  <div class="base-modal-container">
    <div class="mb-4">
      <div>Default:</div>
      <BasePanelError
        message="「権限を設定するユーザー」を選択してください。"
      />
    </div>
    <div class="mb-4 custom-1">
      <div>Container width height:</div>
      <BasePanelError
        message="「権限を設定するユーザー」を選択してください。"
      />
    </div>
    <div class="mb-4">
      <div>Custom panel error:</div>
      <BasePanelError
        icon="ico-company"
        width="1200px"
        height="190px"
        :width-icon="44"
        :height-icon="44"
        message="「権限を設定するユーザー」を選択してください。"
      />
    </div>
    <div class="mb-4">
      <div>On modal:</div>
      <BasePanelError
        message="「権限を設定するユーザー」を選択してください。"
      />
    </div>
  </div>
</template>

<script>
import BasePanelError from '@/components/commonApp/BasePanelError.vue'

export default {
  components: {
    BasePanelError,
  },
  layout: 'empty',
}
</script>
<style lang="scss">
.base-modal-container {
  padding: 25px;
}
.mb-4 {
  margin-bottom: 1rem;
}
.custom-1 {
  width: 800px;
  height: 400px;
  overflow: auto;
}
</style>
