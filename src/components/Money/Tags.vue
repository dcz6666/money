<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li
        @click="toggle(tag)"
        :class="{ selected: selectedTags.indexOf(tag) >= 0 }"
        v-for="tag in tagList"
        :key="tag.id"
      >
        {{ tag.name }}
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
// import store from "@/store/index2";
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { mixins } from 'vue-class-component'
import TagHelper from '@/mixins/TagHelper'

@Component({
  computed: {
    tagList(){
        return this.$store.state.tagList;
    }
  },
})
export default class Tags extends mixins(TagHelper) {
  created(){
    this.$store.commit('fetchTags')
  }
  selectedTags: string[] = [];
  toggle(tag: string) {
    const index = this.selectedTags.indexOf(tag);
    if (index >= 0) {
      this.selectedTags.splice(index, 1);
    } else {
      this.selectedTags.push(tag);
    }
    this.$emit("update:value", this.selectedTags);
  }
  
  // createTag() {
  //   const name = window.prompt("请输入标签名");
  //   if (!name) {
  //     window.alert("标签名不能为空");
  //     return;
  //   }
  //   this.$store.commit('createTag',name)
  // }
}
</script>
<style lang="scss" scoped>
.tags {
  background: white;
  flex-grow: 1;
  font-size: 14px;
  padding: 16px;
  display: flex;
  flex-direction: column-reverse;
  > .current {
    display: flex;
    flex-wrap: wrap;
    > li {
      $bg: #d9d9d9;
      background: $bg;
      $h: 24px;
      height: $h;
      line-height: $h;
      border-radius: ($h/2);
      padding: 0 16px;
      margin-right: 12px;
      margin-top: 4px;
      &.selected {
        background: darken($bg, 50%);
        color: #fff;
      }
    }
  }
  > .new {
    padding-top: 16px;
    button {
      background: transparent;
      border: none;
      color: #999;
      border-bottom: 1px solid;
      padding: 0 4px;
    }
  }
}
</style>
