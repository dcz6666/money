<template>
  <layout>
    <Tabs
      class-prefix="type"
      :data-source="recordTypeList"
      :value.sync="type"
    />
    <!-- <Tabs
      class-prefix="interval"
      :data-source="intervalList"
      :value.sync="interval"
    /> -->
    <ol v-if="groupList.length>0">
      <li v-for="(group, index) in groupList" :key="index">
        <h3 class="title">{{ beautify(group.title)}} <span>¥{{group.total}}</span></h3>
        
        <ol>
          <li class="record" v-for="item in group.items" :key="item.id">
            <span>{{ tagString(item.tags) }}</span>
            <span class="notes">{{ item.notes }}</span>
            <span>￥{{ item.amount }}</span>
          </li>
        </ol>
      </li>
    </ol>
    <div v-else class="noResult">
      目前没有相关记录
    </div>
  </layout>
</template>
<style scoped lang="scss">
%item {
  padding: 8px 16px;
  line-height: 24px;
  display: flex;
  justify-content: space-between;
  align-content: center;
}
.title {
  @extend %item;
}
.record {
  @extend %item;
  background: white;
}
.notes {
  margin-right: auto;
  margin-left: 16px;
  color: #999;
}
::v-deep {
  .type-tabs-item {
    background: #c4c4c4;
    &.selected {
      background: #fff;
      &::after {
        display: none;
      }
    }
  }
  .interval-tabs-item {
    height: 48px;
  }
}
.noResult{
  padding:16px;
  text-align: center;
}
</style>
<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import Tabs from "@/components/Tabs.vue";
import intervalList from "@/constants/intervalList";
import recordTypeList from "@/constants/recordTypeList";
import clone from "@/lib/clone";
import dayjs from "dayjs";
const oneDay = 86400 * 1000;
@Component({
  components: {
    Tabs,
  },
})
export default class Statistics extends Vue {
  beautify(string: string) {
    const day = dayjs(string);
    const now = dayjs();
    if (day.isSame(now, "day")) {
      return "今天";
    } else if (day.isSame(now.subtract(1, "day"), "day")) {
      return "昨天";
    } else if (day.isSame(now.subtract(2, "day"), "day")) {
      return "前天";
    } else if (day.isSame(now, "year")) {
      return day.format("M月D日");
    } else {
      return day.format("YYYY年M月D日");
    }
  }
  beforeCreate() {
    this.$store.commit("fetchRecords");
  }
  tagString(tags: Tag[]) {
    return tags.length === 0 ? "无" : tags.map(t=>t.name) .join(",");
  }
  get recordList() {
    return (this.$store.state as RootState).recordList;
  }
  get groupList() {
    let { recordList } = this;
    // type HashTableValue = { title: string; items: RecordItem[] };
    const newList = clone(recordList)
      .filter(r=> r.type === this.type)
      .sort((a, b) => dayjs(b.createdAt).valueOf() - dayjs(a.createdAt).valueOf());
      if (newList.length === 0) {
      return [] as Result;
    }
    type Result ={title:string, total?:number,items:RecordItem[] }[]
    const result:Result = [
      {
        title: dayjs(newList[0].createdAt).format("YYYY-MM-DD"),
        total:0,
        items: [newList[0]],
      },
    ];
    for (let i = 1; i < newList.length; i++) {
      const current = newList[i];
      const last = result[result.length - 1];
      if (dayjs(last.title).isSame(dayjs(current.createdAt), "day")) {
        last.items.push(current);
      } else {
        result.push({
          title: dayjs(current.createdAt).format("YYYY-MM-DD"),
          items: [current],
        });
      }
    }
  
    result.map(group=>{
      group.total = group.items.reduce((sum,item)=>{
        return sum+item.amount
        },0)
    })
    return result;
  }
  type = "-";
  recordTypeList = recordTypeList;
}
</script>

