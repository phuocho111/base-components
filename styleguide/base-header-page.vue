<template>
  <div class="mt-10">
    <div class="section-cus">
      HEADER
      <BaseHeaderPage
        title="発電予測"
        topClassName="top-grid"
        :is-show-mobile="true"
      ></BaseHeaderPage>
    </div>
    <div class="section-cus">
      HEADER with top slot
      <BaseHeaderPage
        title="発電予測"
        topClassName="top-grid"
        :is-show-mobile="true"
      >
        <template #top>
          <div class="d-none d-sm-block"></div>
          <div class="header-top-building-info">
            <BaseIcon
              :width="22"
              :height="22"
              iconName="ico-company"
              class="mr-2 d-none d-sm-block"
            />
            {{ customerName }}
          </div>
        </template></BaseHeaderPage
      >
    </div>
    <div class="section-cus">
      HEADER WITH full-slot
      <BaseHeaderPage
        title="発電予測"
        topClassName="top-grid"
        :is-show-mobile="true"
      >
        <template #top>
          <div class="d-none d-sm-block"></div>
          <div class="header-top-building-info">
            <BaseIcon
              :width="22"
              :height="22"
              iconName="ico-company"
              class="mr-2 d-none d-sm-block"
            />
            {{ customerName }}
          </div>
        </template>
        <template #bottom>
          <div class="filter-container">
            <div class="filter-item">
              <BaseIcon
                :width="22"
                :height="22"
                iconName="ico-area"
                class="mr-2 d-none d-sm-block"
              />
              <BasePulldown class="common-select" />
            </div>
            <div class="filter-item">
              <BaseIcon
                :width="22"
                :height="22"
                iconName="ico-power-plan"
                class="mr-2 d-none d-sm-block"
              />
              <BasePulldown class="common-select" />
            </div>
            <div class="filter-item">
              <BaseIcon
                :width="22"
                :height="22"
                iconName="ico-factory"
                class="mr-2 d-none d-sm-block"
              />
              <BasePulldown class="common-select" />
            </div>
          </div>
        </template>
      </BaseHeaderPage>
    </div>
    <div class="section-cus">
      HEADER with top-slot (select + text)
      <BaseHeaderPage title="発電予測" topTemplateColumn="1fr 1fr auto">
        <template #top>
          <div class="filter-item d-flex w-100">
            <BaseIcon
              :width="22"
              :height="22"
              iconName="ico-area"
              class="mr-2 d-none d-sm-block"
            />
            <BasePulldown class="common-select" />
          </div>
          <div class="d-flex flex-column flex-start fit-content font-12">
            <div class="">
              {{ customerName }}
            </div>
            <div>2022/11/22 12:34</div>
          </div>
        </template>
      </BaseHeaderPage>
    </div>
    <div class="section-cus">
      <BaseHeaderPage
        title="補助金情報"
        topTemplateColumn="1fr 1fr auto"
        :isWrapMobile="true"
      >
        <template #top>
          <div class="d-none d-sm-block"></div>
          <div class="d-flex flex-column flex-start fit-content font-12">
            <div class="font-12">最終更新</div>
            <div class="font-12">2022/11/22 12:34</div>
          </div>
        </template>
      </BaseHeaderPage>
    </div>
  </div>
</template>
<script>
import BasePulldown from '~/components/commonApp/BasePulldown.vue'
import BaseHeaderPage from '~/components/commonApp/BaseHeaderPage.vue'
import BaseIcon from '~/components/commonApp/BaseIcon.vue'
export default {
  components: {
    BasePulldown,
    BaseHeaderPage,
    BaseIcon,
  },
  layout: 'empty',
  data: () => ({
    totalPage: 100,
    currentPage: 1,
    customerName: '最終更新',
  }),
}
</script>
<style lang="scss" scoped>
.section-cus {
  padding: 15px 20px;
}
.fit-content {
  width: fit-content;
}

.w-385 {
  width: 385px;
}

.custom {
  width: 80%;
  justify-content: space-between;
  > * {
    width: calc(50% - 50px);
    display: flex;
    justify-content: center;
  }
}

.header-top-building-info {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  font-size: 13px;
  letter-spacing: 0.32px;
  border: 1px solid $mirage_color;
  padding: 8px 10px;
}

.filter-container {
  min-height: 36px;
  display: grid;
  gap: 16px;
  grid-template-columns: repeat(3, 1fr);
}
.filter-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.header-custom-select {
  padding-top: 0 !important;
  margin-top: 0 !important;
}
.common-select {
  width: 100% !important;
  ::v-deep .v-text-field {
    margin-top: 0 !important;
    padding: 0px !important;
    input {
      padding: 0;
      height: 24px !important;
      max-height: 24px !important;
    }
  }
}
</style>
