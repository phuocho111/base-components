<template>
  <div class="p-2">
    NOTE: Filter
    <br />
    <BaseSwitch
      v-model="isShowHistory"
      label="発電契約者が紐づいていない発電所のみ表示"
    />
    <div class="custom">
      <BaseTreeView
        :data-tree="treeData"
        :has-check-box="true"
        :is-show-history="isShowHistory"
      />
    </div>
  </div>
</template>

<script>
import BaseTreeView from '@/components/commonApp/BaseTreeView.vue'
import BaseSwitch from '@/components/commonApp/BaseSwitch.vue'

export default {
  components: {
    BaseTreeView,
    BaseSwitch,
  },
  layout: 'empty',
  data: () => ({
    isShowHistory: false,
    treeData: [
      {
        id: 1,
        label: '株式会社四電工',
        isDisabled: false,
        isChecked: false,
        children: [
          {
            id: 2,
            label: '四国',
            isDisabled: true,
            isChecked: true,
            children: [
              {
                id: 3,
                label: 'サンシャインパーク久礼',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 4,
                label: 'サンシャインパーク佐川',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 5,
                label: 'サンシャインパーク加茂',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 6,
                label: 'サンシャインパーク北条',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 7,
                label: 'サンシャインパーク大屋冨',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 8,
                label: 'サンシャインパーク箸蔵',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
              {
                id: 9,
                label: 'サンシャインパーク豊浜',
                children: [],
                isDisabled: true,
                isChecked: true,
              },
            ],
          },
          {
            label: '中国',
            id: 10,
            isDisabled: false,
            isChecked: false,
            children: [
              {
                id: 11,
                label: 'もみの木サンシャインパーク',
                children: [],
                isDisabled: false,
                isChecked: false,
              },
              {
                id: 12,
                label: 'もみの木サンシャインパーク 12',
                isDisabled: false,
                isChecked: false,
                children: [
                  {
                    id: 13,
                    label: 'もみの木サンシャインパーク4',
                    children: [],
                    isDisabled: false,
                    isChecked: true,
                  },
                  {
                    id: 14,
                    label: 'もみの木サンシャインパーク4',
                    children: [],
                    isDisabled: false,
                    isChecked: false,
                  },
                ],
              },
              {
                id: 15,
                label: 'もみの木サンシャインパーク',
                isDisabled: false,
                isChecked: false,
                children: [
                  {
                    id: 16,
                    label: 'もみの木サンシャインパーク4',
                    children: [],
                    isDisabled: false,
                    isChecked: false,
                  },
                  {
                    id: 17,
                    label: 'もみの木サンシャインパーク4',
                    children: [],
                    isDisabled: false,
                    isChecked: false,
                  },
                ],
              },
            ],
          },
          {
            id: 18,
            label: 'ベトナム',
            isDisabled: false,
            isChecked: false,
            children: [
              {
                id: 19,
                label: 'もみの木サンシャインパーク1',
                children: [],
                isDisabled: false,
                isChecked: false,
              },
              {
                id: 20,
                label: 'もみの木サンシャインパーク1',
                isDisabled: false,
                children: [
                  {
                    id: 21,
                    label: 'もみの木サンシャインパーク1',
                    children: [],
                    isDisabled: false,
                    isChecked: false,
                  },
                ],
              },
            ],
          },
        ],
      },
      {
        id: 22,
        label: 'ニトリ株式会社',
        isDisabled: false,
        isChecked: false,
        children: [
          {
            id: 23,
            label: '東京',
            isDisabled: false,
            isChecked: false,
            children: [
              {
                id: 24,
                label: '発電所A',
                children: [],
                isDisabled: false,
                isChecked: true,
              },
              {
                id: 25,
                label: '発電所B',
                children: [],
                isDisabled: false,
                isChecked: true,
              },
              {
                id: 26,
                label: '発電所C',
                isDisabled: false,
                isChecked: false,
                children: [
                  {
                    id: 31,
                    label: '発電所G',
                    children: [],
                    isDisabled: false,
                    isChecked: true,
                  },
                  {
                    id: 32,
                    label: '発電所H',
                    isDisabled: false,
                    children: [
                      {
                        id: 33,
                        label: '発電所I',
                        children: [],
                        isDisabled: false,
                        isChecked: true,
                      },
                      {
                        id: 34,
                        label: '発電所J',
                        children: [],
                        isDisabled: false,
                        isChecked: false,
                      },
                    ],
                  },
                ],
              },
            ],
          },
          {
            id: 27,
            label: '関西',
            isDisabled: false,
            isChecked: false,
            children: [
              {
                id: 28,
                label: '発電所B',
                children: [],
                isDisabled: false,
                isChecked: true,
              },
              {
                id: 29,
                label: '発電所E',
                children: [],
                isDisabled: false,
                isChecked: false,
              },
              {
                id: 30,
                label: '発電所F',
                children: [],
                isDisabled: false,
                isChecked: true,
              },
            ],
          },
        ],
      },
    ],
  }),
}
</script>

<style lang="scss" scoped>
.p-2 {
  padding: 2rem;
}

.custom {
  width: 100%;
  height: 300px;
  overflow: auto;
}
</style>
