<template>
  <div class="ml-4 mt-4">
    <div class="pull-down_width-200">
      <h2>Normal Select</h2>
      <BasePulldown v-model="select1" class="common-select" :items="items" />

      <h2 class="mt-5">preset data ( v-model data)</h2>
      <BasePulldown v-model="select1" class="common-select" :items="items" />

      <h2 class="mt-5">select multiple</h2>
      <BasePulldown
        v-model="selectMultiple"
        class="common-select"
        multiple
        :items="items"
      />

      <h2 class="mt-5">placeholder ( nodata)</h2>
      <BasePulldown
        v-model="select3"
        class="common-select"
        :items="items"
        placeholder="選択してください"
      />

      <h2 class="mt-5">Error Select</h2>
      <BasePulldown
        v-model="select2"
        class="common-select"
        :items="items"
        error
        errorMessage="エラーメッセージ"
      />

      <h2 class="mt-5">Select has Options Disabled</h2>
      <BasePulldown
        v-model="select4"
        class="common-select"
        :items="itemsIsDisabled"
      />

      <h2 class="mt-5">Readonly Select (can't select option)</h2>
      <BasePulldown
        v-model="select6"
        class="common-select"
        :items="items"
        :isDisabled="true"
      />
    </div>

    <hr class="mt-10" />
    <div class="pull-down_max-width">
      <h2>Select Full Width</h2>
      <BasePulldown v-model="select8" class="common-select" :items="items">
      </BasePulldown>

      <h2 class="mt-5">Error Select</h2>
      <BasePulldown
        v-model="select2"
        class="common-select"
        :items="items"
        error
        errorMessage="エラーメッセージ"
      />
    </div>

    <hr class="mt-10" />
    <h2>ADVANCE</h2>
    <div class="pull-down_width-300">
      <h2>+ Has Add, Remove, Select All option</h2>
      <BasePulldown
        v-model="selectfunction"
        class="common-select"
        :items="items"
        showAddOption
        multiple
        showDeleteOptions
        showSelectAll
        @addOption="onAddOption"
        @deleteOption="onDeleteOption"
        @selectAllOption="onSelectAllOption"
      />

      <h2>Select Disabled have add Item fuction</h2>
      <BasePulldown
        v-model="select5"
        class="common-select"
        :items="itemsIsDisabled"
        showDeleteOptions
        @addOption="onAddOption"
        @deleteOption="onDeleteOption"
      />
    </div>
  </div>
</template>
<script>
import BasePulldown from '@/components/commonApp/BasePulldown.vue'
export default {
  components: { BasePulldown },
  layout: 'empty',
  data() {
    return {
      select1: null,
      select2: null,
      select3: null,
      select4: null,
      select5: null,
      select6: '1',
      select7: null,
      select8: null,
      selectX: null,
      selectMultiple: null,
      selectfunction: null,
      items: [
        {
          text: 'foo',
          value: '1',
        },
        {
          text: 'bar',
          value: '2',
        },
        {
          text: 'fizz',
          value: '3',
        },
        {
          text: 'buzz',
          value: '4',
        },
      ],
      itemsIsDisabled: [
        {
          text: 'foo',
          value: 'foo',
          disabled: true,
        },
        {
          text: 'bar',
          value: 'bar',
        },
        {
          text: 'fizz',
          value: 'fizz',
          disabled: true,
        },
        {
          text: 'buzz',
          value: 'buzz',
        },
      ],
    }
  },
  computed: {
    selectAllItems() {
      return this.selectfunction === this.items
    },
  },
  methods: {
    onAddOption() {
      console.log('create')
    },

    onDeleteOption(item) {
      console.log('onDeleteOption')
      // demo spec delete option
      const deleteItem = this.items.indexOf(item)
      this.items.splice(deleteItem, 1)
      if (this.selectfunction instanceof Array) {
        const removeSelect = this.selectfunction.indexOf(item)
        this.selectfunction.splice(removeSelect, 1)
      } else {
        this.selectfunction = null
      }
    },
    onSelectAllOption() {
      console.log('onSelectAllOption')
      // demo spec select all
      if (this.selectAllItems) {
        this.selectfunction = []
      } else {
        this.selectfunction = this.items
      }
    },
  },
}
</script>
<style lang="scss" scoped>
.pull-down_width-300 {
  width: 300px;
  margin-bottom: 20px;
}
.pull-down_width-200 {
  width: 200px;
  margin-bottom: 20px;
}
.pull-down_max-width {
  width: 100%;
}
</style>
