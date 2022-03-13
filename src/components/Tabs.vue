<template>
  <div>
    <ul class="tabs" :class="{[classPrefix+'tabs']:classPrefix}">
      <li
        v-for="(item, index) in dataSource"
        :key="index"
        @click="select(item)"
        class="tabs-item"
        :class="liClass(item)"
      >
        {{ item.text }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
type DataSourceItem = { text: string; value: string };
@Component
export default class Tabs extends Vue {
  @Prop({ required: true, type: Array }) dataSource!: DataSourceItem[];
  @Prop(String) readonly value!: string;
  @Prop(String) classPrefix?: string;

  liClass (item:DataSourceItem) {
    return {
      [this.classPrefix + "-tabs-item"]: this.classPrefix,
      selected: item.value === this.value,
    };
  };
  select(item: DataSourceItem) {
    console.log("item", item);
    this.$emit("update:value", item.value);
  }
}
</script>

<style lang='scss' scoped>
.tabs {
  background: #c4c4c4;
  display: flex;
  text-align: center;
  >.tabs-item {
    width: 50%;
    line-height: 64px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    &.selected::after {
      content: "";
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 4px;
      background: #333;
    }
  }
}
</style>