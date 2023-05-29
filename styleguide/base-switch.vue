<template>
  <div class="ml-4 mt-4">
    <div class="switch-container">
      <h2>Switch no label</h2>
      <p>Value: {{ value1 }}</p>
      <BaseSwitch v-model="value1" />
    </div>
    <div class="switch-container">
      <h2>Switch label</h2>
      <p>Value: {{ value2 }}</p>
      <BaseSwitch v-model="value2" label="Switch 2" />
    </div>
    <div class="switch-container">
      <h2>Switch disabled</h2>
      <BaseSwitch :value="true" label="Switch 3" disabled />
      <BaseSwitch :value="false" label="Switch 3" disabled />
    </div>
  </div>
</template>
<script>
import BaseSwitch from '@/components/commonApp/BaseSwitch.vue'
export default {
  components: { BaseSwitch },
  layout: 'empty',
  data: () => ({
    value1: false,
    value2: true,
  }),
  computed: {},
  methods: {},
}
</script>
<style lang="scss" scoped>
.switch-container {
  margin-bottom: 30px;
}

p {
  margin-bottom: 0;
  padding-bottom: 0;
}
</style>
