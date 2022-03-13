<template>
  <div class="nav-wrapper">
    <layout classPrefix="layout">
      <NumberPad :value.sync="record.amount" @submit="saveRecode" />
      <Tabs
        class-prefix="type"
        :data-source="recordTypeList"
        :value.sync="record.type"
      />
      <div class="notes">
        <FormItem
          field-name="备注"
          placeholder="在这里输入备注"
          :value.sync="record.notes"
        />
      </div>
      <Tags @update:value="record.tags = $event" />
    </layout>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Watch } from "vue-property-decorator";
import NumberPad from "@/components/Money/NumberPad.vue";
import FormItem from "@/components/Money/FormItem.vue";
import Tags from "@/components/Money/Tags.vue";
import Tabs from "@/components/Tabs.vue";
import recordTypeList from "@/constants/recordTypeList";

@Component({
  components: { NumberPad, Tags, FormItem, Tabs },
})
export default class Money extends Vue {
  get recordList() {
    return this.$store.state.recordList;
  }
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0,
  };
  recordTypeList = recordTypeList;
  created() {
    this.$store.commit("fetchRecords");
  }
  onUpdateNots(value: string) {
    this.record.notes = value;
  }
  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }
  saveRecode() {
    if(!this.record.tags || this.record.tags.length==0){
      return window. alert("请至少选择一个标签")
    }
    this.$store.commit("createRecord", this.record);
    if(this.$store.state.createRecordError===null){
      window.alert("已保存")
      this.record.notes=""
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~@/assets/style/helper.scss";
  ::v-deep .layout-content {
    display: flex;
    flex-direction: column-reverse;
  }
  .notes {
    padding: 12px 0;
  }
</style>


