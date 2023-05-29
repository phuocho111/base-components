<template>
  <div class="ma-8">
    <div class="cards">
      <h1 class="title-card">Default</h1>
      <div class="card-types">
        <div class="mt-4">
          <h2 class="mb-2">Card day</h2>
          <BaseCard
            period="翌日"
            date="2023/03/24"
            statusVariant="status-day"
            itemName="発電BG1"
          />
        </div>
        <div class="mt-4">
          <h2 class="mb-2">Card Week</h2>
          <BaseCard
            period="週間"
            date="2023/03/25"
            statusVariant="status-week"
            itemName="発電BG2"
          />
        </div>
        <div class="mt-4">
          <h2 class="mb-2">Card month</h2>
          <BaseCard
            period="月間"
            date="2023/03/26"
            statusVariant="status-month"
            itemName="発電BG1"
          />
        </div>
        <div class="mt-4">
          <h2 class="mb-2">Card year</h2>
          <BaseCard
            period="年間"
            date="2023/03/27"
            statusVariant="status-year"
            itemName="発電BG5"
          />
        </div>
      </div>
    </div>
    <div class="cards">
      <h1 class="title-card">Card Submited</h1>
      <div class="card-types">
        <div class="mt-4">
          <BaseCard
            period="翌日"
            date="2023/03/24"
            statusVariant="status-day"
            itemName="発電BG1"
            isSubmited
          />
        </div>
      </div>
    </div>
    <div class="cards">
      <h1 class="title-card">Card has remove automatic and display intraday</h1>
      <div class="card-types">
        <div class="mt-4">
          <BaseCard
            period="翌日"
            date="2023/03/24"
            statusVariant="status-day"
            itemName="発電BG1"
            showRemoveAuto
            showIntraday
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import BaseCard from '~/components/commonApp/BaseCard.vue'
export default {
  components: { BaseCard },
  data() {
    return {}
  },
}
</script>
<style lang="scss" scoped>
.cards {
  max-width: 900px;
  display: flex;
  flex-direction: column;
  margin-top: 40px;
  .title-card {
    font-size: 20px;
    color: white;
    font-weight: 700;
  }
  .card-types {
    min-width: 136px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    .base-card {
      min-width: 136px;
    }
  }
}
</style>
