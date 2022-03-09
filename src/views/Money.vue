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
      <Tags/>
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

@Component({
  components: { NumberPad, Types, Tags, FormItem },
  computed:{
    recordList(){
      return this.$store.state.recordList;
    }
  }
})

export default class Money extends Vue {
  // recordList: RecordItem[] | undefined = oldStore.recordList;
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0,
  };

  created(){
    this.$store.commit('fetchRecords')
  }
  onUpdateNots(value: string) {
    this.record.notes = value;
  }
  onUpdateAmount(value: string) {
    this.record.amount = parseFloat(value);
  }
  saveRecode() {
    this.$store.commit('createRecord',this.record);
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


