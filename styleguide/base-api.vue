<template>
  <div class="w-100 pa-10">
    <BaseInput v-model="url" type="text" placeholder="API_URL" />
    <br />
    <BaseInput
      v-model="method"
      type="text"
      placeholder="method: get put post delete"
    />
    <br />
    <BaseInput
      v-model="payload"
      type="text"
      placeholder="payload { key:value, key:value}"
    />
    <br />
    <NewButton class="linear-gradient mt-10 w-100" @action="handleClick"
      >CALL TEST</NewButton
    >
    <br />
    <br />
    Response:
    <NewButton
      class="linear-gradient float-right"
      height="26"
      @action="copyText"
      >copy</NewButton
    >
    <hr />
    <div class="response-container">
      <pre class="json-data">{{ JSON.stringify(response, null, 4) }}</pre>
    </div>
  </div>
</template>

<script>
import NewButton from '~/components/commonApp/NewButton.vue'
import BaseInput from '~/components/commonApp/BaseInput.vue'
export default {
  components: {
    NewButton,
    BaseInput,
  },
  layout: 'empty',
  data() {
    return {
      url: '/auth/v1/users/profile',
      method: 'get',
      payload: '',
      response: {},
    }
  },
  methods: {
    copyText() {
      const el = document.createElement('textarea')
      el.value = JSON.stringify(this.response, null, 4)
      document.body.appendChild(el)
      el.select()
      document.execCommand('copy')
      document.body.removeChild(el)
      alert('Text copied to clipboard!')
    },
    async handleClick() {
      switch (this.method) {
        case 'get':
          this.response = await this.$axios.$get(this.url)
          console.log(this.response)
          break
        case 'post':
          this.response = await this.$axios.$post(
            this.url,
            JSON.parse(this.payload)
          )
          break

        case 'put':
          this.response = await this.$axios.$put(
            this.url,
            JSON.parse(this.payload)
          )
          break
        case 'delete':
          this.response = await this.$axios.$delete(
            this.url,
            JSON.parse(this.payload)
          )
          break

        default:
          break
      }
    },
  },
}
</script>
<style lang="scss" scoped>
.float-right {
  float: right;
}
.response-container {
  max-height: 46vh;
  overflow: auto;

  .json-data {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 13px;
  }
}
</style>
