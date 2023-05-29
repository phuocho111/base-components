<template>
  <div class="d-flex justify-space-around">
    <div class="d-flex flex-column">
      <h1>Input Text</h1>
      <BaseInput v-model="username" type="text" placeholder="ユーザーID" />

      <h1 class="mt-30">Input error</h1>
      <BaseInput placeholder="ユーザーID" error :errMsg="errMsg" />

      <h1 class="mt-30">Input with subfix</h1>
      <BaseInput label="ユーザーID" subfixText="％" />

      <h1 class="mt-30">Input Search</h1>
      <BaseInput
        v-model="searchText"
        type="search"
        @onClickSubfix="handleClick"
      />

      <h1 class="mt-30">Input Search</h1>
      <BaseInput
        v-model="searchText"
        type="search"
        error
        :errMsg="errMsg"
        @onClickSubfix="handleClick"
      />

      <h1 class="mt-30">Input password</h1>
      <BaseInput
        v-model="password"
        type="password"
        @handleEnterKeyup="handleLogin"
      />
    </div>
    <div>
      <h1>Input Cell Number value</h1>
      <BaseInputCell
        v-model="cellInputNumberValue"
        placeholder="N/A"
        type="number"
        @blur="handleBlurNumber"
      />

      <h1 class="mt-30">Input Cell Text value</h1>
      <BaseInputCell v-model="cellInputTextValue" placeholder="N/A" />

      <h1 class="mt-30">Input Cell error</h1>
      <BaseInputCell
        v-model="cellInputTextValue"
        placeholder="N/A"
        :error="isErrorCellInput"
      />
    </div>
  </div>
</template>

<script>
import BaseInput from '@/components/commonApp/BaseInput.vue'
import BaseInputCell from '@/components/commonApp/BaseInputCell.vue'

export default {
  components: { BaseInput, BaseInputCell },
  layout: 'empty',
  data() {
    return {
      username: '',
      errMsg: 'エラーメッセージ',
      searchText: '',
      password: '',
      cellInputNumberValue: null,
      cellInputTextValue: '',
      isErrorCellInput: true,
    }
  },
  methods: {
    // event for BaseInput
    handleClick() {
      console.log('SearchText', this.searchText)
    },
    handleLogin() {
      console.log('handle login', this.password)
    },
    // event for BaseCellInput example
    handleBlurNumber() {
      console.log('handle blur')
    },
  },
}
</script>
