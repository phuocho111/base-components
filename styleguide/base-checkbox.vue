<template>
  <div>
    <h2 class="mt-4">Default</h2>
    <BaseCheckbox v-model="checked1" label="label" />

    <h2 class="mt-4">No label</h2>
    <BaseCheckbox v-model="checked2" />

    <h2 class="mt-4">Disabled</h2>
    <BaseCheckbox v-model="checked3" disabled />
    <BaseCheckbox v-model="checked4" disabled />

    <h2 class="mt-4">Group</h2>
    <div class="check-group">
      <template v-for="(item, index) in checkGroup">
        <BaseCheckbox :key="index" v-model="checkGroup[index]" />
      </template>
    </div>

    <h2 class="mt-4">Group Horizontal and Labeled</h2>
    <div class="check-group horizontal">
      <template v-for="(item, index) in checkGroup2">
        <BaseCheckbox
          :key="index"
          v-model="checkGroup2[index].value"
          :label="checkGroup2[index].label"
          :disabled="checkGroup2[index].disabled"
        />
      </template>
    </div>

    <h2 class="mt-4">Custom label use slot</h2>
    <BaseCheckbox v-model="checked1" label="44444" />
    <BaseCheckbox v-model="checked1">
      <template #label>
        <span>444<b class="red--text">redtext</b>4</span>
      </template>
    </BaseCheckbox>
  </div>
</template>
<script>
import BaseCheckbox from '~/components/commonApp/BaseCheckbox.vue'
export default {
  components: { BaseCheckbox },
  layout: 'empty',
  data() {
    return {
      checked1: true,
      checked2: false,
      checked3: false,
      checked4: true,
      checkGroup: {
        key1: true,
        key2: false,
        key3: false,
        key4: false,
      },
      checkGroup2: {
        key1: {
          value: true,
          label: '11111',
        },
        key2: {
          value: false,
          label: '22222',
          disabled: true,
        },
        key3: {
          value: true,
          label: '33333',
          disabled: true,
        },
        key4: {
          value: true,
          label: '44444',
          disabled: false,
        },
      },
    }
  },
}
</script>
<style lang="scss" scoped>
.check-group {
  display: flex;
  column-gap: 10px;
  &.horizontal {
    flex-direction: column;
    row-gap: 10px;
  }
}
</style>
