<template>
  <div class="d-flex flex-row justify-space-around">
    <div class="d-flex flex-column column-block">
      <h1>Variant</h1>
      <div class="pb-2 mt-5">
        <h2>Primary</h2>
        <NewButton class="linear-gradient" @action="handleClick"
          >テキスト</NewButton
        >
        <v-spacer></v-spacer>
      </div>
      <!-- <div class="pb-2">
        <h2>Primary+Icon</h2>
        <NewButton class="linear-gradient" @action="handleClick">
          <img src="@/static/images/Icons/7_Button/Icon_Button_Save.svg" alt="" />
          テキスト
        </NewButton>
      </div> -->
      <div class="pb-2">
        <h2>Secondary</h2>
        <NewButton class="default" @action="handleClick">テキスト</NewButton>
      </div>
      <div class="pb-2">
        <h2>Secondary, Secondary + icon</h2>
        <NewButton class="default none-active" @action="handleClick"
          ><img
            src="@/static/images/Icons/7_Button/Icon_Button_Calendar.svg"
            alt=""
            class="button-icon"
          />テキスト</NewButton
        >
      </div>
      <div>
        <h2>Secondary/IconOnly</h2>
        <NewButton class="default none-active" @action="handleClick">
          <img
            src="@/static/images/Icons/7_Button/Icon_Button_BackArrow.svg"
            alt=""
            class="button-icon"
          />
        </NewButton>
      </div>
      <div class="pb-2">
        <h2>Button For Table Cell</h2>
        <NewButton height="26" class="default none-active" @action="handleClick"
          >テキスト</NewButton
        >
      </div>
    </div>
    <div class="d-flex flex-column column-block">
      <h1>Disable status</h1>
      <div class="pb-2 mt-5">
        <h2>Primary & Secondary Disabled</h2>
        <NewButton :class="{ disable: isDisable }" @action="handleClick"
          >テキスト</NewButton
        >
      </div>
      <div class="pb-2">
        <h2>Primary+Icon & Secondary + icon Disable</h2>
        <NewButton :class="{ disable: isDisable }" @action="handleClick"
          ><img
            src="@/static/images/Icons/7_Button/Icon_Button_Calendar.svg"
            alt=""
            class="button-icon"
          />テキスト</NewButton
        >
      </div>
      <div>
        <h2>Secondary/IconOnly</h2>
        <NewButton
          class="default none-active"
          :class="{ 'disable disable-icon-only': isDisable }"
        >
          <img
            src="@/static/images/Icons/7_Button/Icon_Button_BackArrow.svg"
            alt=""
            class="button-icon"
            @action="handleClick"
          />
        </NewButton>
      </div>
      <div class="pb-2">
        <h2>Button For Table Cell</h2>
        <NewButton
          height="26"
          class="default none-active"
          :class="{ 'disable disable-table-cell': isDisable }"
          @action="handleClick"
          >テキスト</NewButton
        >
      </div>
    </div>
    <div class="d-flex flex-column column-block">
      <div class="pb-2">
        <h1>Fixed width</h1>
        <h2 class="mt-5">Primary & Secondary</h2>
        <NewButton class="linear-gradient" :width="150" @action="handleClick"
          >テキスト</NewButton
        >
      </div>
      <div class="pb-2">
        <h2>Secondary, Secondary + icon</h2>
        <NewButton
          class="default none-active"
          :width="150"
          @action="handleClick"
          ><img
            src="@/static/images/Icons/7_Button/Icon_Button_Calendar.svg"
            alt=""
            class="button-icon"
          />テキスト</NewButton
        >
      </div>
      <div class="pb-2">
        <h2>Button For Table Cell</h2>
        <NewButton
          height="26"
          class="default none-active"
          :width="150"
          @action="handleClick"
          >テキスト</NewButton
        >
      </div>
    </div>
    <div class="d-flex flex-column column-block">
      <h1>Full witdh</h1>
      <div class="pb-2 mt-5">
        <h2>Primary & Secondary</h2>
        <NewButton class="linear-gradient" width="100%" @action="handleClick"
          >テキスト</NewButton
        >
      </div>
      <div class="pb-2">
        <h2>Secondary, Secondary + icon</h2>
        <NewButton
          class="default none-active"
          width="100%"
          @action="handleClick"
          ><img
            src="@/static/images/Icons/7_Button/Icon_Button_Calendar.svg"
            alt=""
            class="button-icon"
          />テキスト</NewButton
        >
      </div>
      <div class="pb-2">
        <h2>Button For Table Cell</h2>
        <NewButton
          height="26"
          class="default none-active"
          width="100%"
          @action="handleClick"
          >テキスト</NewButton
        >
      </div>
    </div>
  </div>
</template>
<script>
import NewButton from '~/components/commonApp/NewButton.vue'
export default {
  components: { NewButton },
  layout: 'empty',
  data() {
    return {
      isDisable: true,
    }
  },
  methods: {
    handleClick() {
      console.log('handle Click')
    },
  },
}
</script>

<style lang="scss" scoped>
.column-block {
  width: 300px;
}
</style>
