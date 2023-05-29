<template>
  <div class="mt-10 ml-10">
    <div class="d-flex mt-5">
      <div class="mr-5 w-300">Pagination No Page:</div>
      <BasePagination v-model="currentPage" :totalPage="0" :currentPage="0" />
    </div>
    <hr class="mt-5" />
    <div class="d-flex mt-5">
      <div class="mr-5 w-300">Pagination No Page with show:</div>
      <BasePagination
        v-model="currentPage1"
        :totalPage="0"
        :currentPage="0"
        :alwaysShow="true"
      />
    </div>
    <hr class="mt-5" />
    <div class="d-flex justify-start mt-5">
      <div class="mr-5 w-300">Pagination full-width:</div>
      <BasePagination v-model="currentPage" :totalPage="totalPage" />
    </div>
    <hr class="mt-5" />
    <div class="d-flex mt-5">
      <div class="mr-5 w-300">Pagination with custom size:</div>
      <BasePagination
        v-model="currentPage"
        :totalPage="totalPage"
        width="425px"
      />
    </div>
    <hr class="mt-5" />
    <div class="d-flex mt-5">
      <div class="mr-5 w-300">Pagination with custom size:</div>
      <div class="d-flex w-100 custom">
        <BasePagination v-model="currentPage" :totalPage="totalPage" />
        <div class="align-center bg-yellow">50%</div>
      </div>
    </div>
    <hr class="mt-5" />
    <div class="d-flex mt-5">
      <div class="mr-5 w-300">Pagination with button:</div>
      <div class="d-flex w-100">
        <BasePagination v-model="currentPage" :totalPage="totalPage">
          <template slot="content">
            <NewButton class="linear-gradient" width="100%">テキスト</NewButton>
          </template>
        </BasePagination>
      </div>
    </div>
  </div>
</template>
<script>
import BasePagination from '~/components/commonApp/BasePagination.vue'
import NewButton from '~/components/commonApp/NewButton.vue'
export default {
  components: {
    BasePagination,
    NewButton,
  },
  layout: 'empty',
  data: () => ({
    totalPage: 100,
    currentPage: 1,
    currentPage1: 1,
  }),
}
</script>
<style lang="scss" scoped>
.w-300 {
  width: 300px;
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

.bg-yellow {
  background: yellow;
}
</style>
