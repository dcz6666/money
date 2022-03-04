<template>
  <div class="nav-wrapper">
    <layout classPrefix="layout">
      <NumberPad @update:value="onUpdateAmount" @submit="saveRecode" />
      <Types :value.sync="record.type" />
      <div class="notes">
        <FormItem
          field-name="备注"
          placeholder="在这里输入备注"
          @update:value="onUpdateNots"
        />
      </div>
      <Tags :data-source.sync="tags" @update:value="onUpdateTags" />
      <!-- {{record}} -->
    </layout>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Watch } from "vue-property-decorator";
import NumberPad from "@/components/Money/NumberPad.vue";
import Types from "@/components/Money/Types.vue";
import FormItem from "@/components/Money/FormItem.vue";
import Tags from "@/components/Money/Tags.vue";

const { recordListModel } = require("@/models/recordListModel.ts");
const { tagListModel } = require("@/models/tagListModel.ts");
const recordList = recordListModel.fetch();

@Component({
  components: { NumberPad, Types, Tags, FormItem },
})
export default class Money extends Vue {
  tags = window.tagList;
  recordList: RecordItem[] = recordList;
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0,
  };
  onUpdateTags(value: string[]) {
    this.record.tags = value;
  }
  onUpdateNots(value: string) {
    this.record.notes = value;
  }
  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }
  saveRecode() {
    recordList.create(this.record)
  }
  @Watch("recordList")
  onRecordListChanged() {
    recordListModel.save();
  }
}
</script>
 <style lang="scss">
.layout-content {
  display: flex;
  flex-direction: column-reverse;
}
.notes{
  padding:12px 0;
}
</style>

<style lang="scss" scoped>
@import "~@/assets/style/helper.scss";
</style>


