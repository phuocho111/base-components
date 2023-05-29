<template>
  <div class="mt-10">
    <BaseTab v-model="tab" :tabItems="items" />
  </div>
</template>
<script>
import BaseTab from '~/components/commonApp/BaseTab.vue'
export default {
  components: { BaseTab },
  data() {
    return {
      items: [
        { name: '北海道', value: 1 },
        { name: '東北', value: 2 },
        { name: '東京', value: 3 },
        { name: '中部', value: 4 },
        { name: '北陸', value: 5 },
        { name: '関西', value: 6 },
        { name: '中国', value: 7 },
        { name: '四国', value: 8 },
        { name: '九州', value: 9 },
        { name: '沖縄', value: 10 },
      ],
      tab: 1,
    }
  },
  computed: {},
}
</script>
