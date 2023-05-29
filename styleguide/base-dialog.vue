<template>
  <div>
    <div class="variant">
      <h2>Variant</h2>
      <hr />
      <p>warning</p>
      <div class="dialog1">
        <BaseButton class="" text="Demo" @action="handleClick(1)" />
        <BaseDialog
          :visible="dialog1"
          submitLabel="オフにする"
          cancelLabel="キャンセル"
          variant="warning"
          @submit="handleSubmit"
          @close="handleCancel"
        />
      </div>
      <p>info</p>
      <div class="dialog2">
        <BaseButton class="" text="Demo" @action="handleClick(2)" />
        <BaseDialog
          :visible="dialog2"
          submitLabel="text"
          cancelLabel="text"
          variant="info"
          @close="handleCancel"
        />
      </div>
      <p>info</p>
      <div class="dialog2">
        <BaseButton class="" text="Demo" @action="handleClick(6)" />
        <BaseDialog
          :visible="dialog6"
          submitLabel="text"
          cancelLabel="text"
          variant="loading"
          @close="handleCancel"
        />
      </div>
    </div>

    <div>
      <h2>Action</h2>
      <hr />
      <p>submit+cancel</p>
      <div>
        <BaseButton class="" text="Demo" @action="handleClick(3)" />
        <BaseDialog
          :visible="dialog3"
          submitLabel="text"
          cancelLabel="text"
          @submit="handleSubmit"
          @close="handleCancel"
        />
      </div>
      <p>submit only</p>
      <div>
        <BaseButton class="" text="Demo" @action="handleClick(4)" />
        <BaseDialog
          :visible="dialog4"
          submitLabel="text"
          @submit="handleSubmit"
          @close="handleCancel"
        />
      </div>
      <p>no action</p>
      <div>
        <BaseButton class="" text="Demo" @action="handleClick(5)" />
        <BaseDialog :visible="dialog5" variant="loading" />
      </div>
    </div>
  </div>
</template>
<script>
import BaseDialog from '@/components/commonApp/BaseDialog.vue'
import BaseButton from '@/components/commonApp/BaseButton.vue'
export default {
  components: { BaseDialog, BaseButton },
  layout: 'empty',
  data() {
    return {
      dialog1: false,
      dialog2: false,
      dialog3: false,
      dialog4: false,
      dialog5: false,
      dialog6: false,
    }
  },
  computed: {},
  methods: {
    handleSubmit() {
      console.log('click')
      this.dialog1 = false
      this.dialog2 = false
      this.dialog3 = false
      this.dialog4 = false
      this.dialog5 = false
      this.dialog6 = false
    },
    handleCancel() {
      this.dialog1 = false
      this.dialog2 = false
      this.dialog3 = false
      this.dialog4 = false
      this.dialog5 = false
      this.dialog6 = false
    },
    handleClick(i) {
      this[`dialog${i}`] = true
    },
  },
}
</script>
