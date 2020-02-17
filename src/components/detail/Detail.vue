<template>
  <div class="container">
    <div class="top">截止至{{allData.date}}(北京时间)统计</div>
    <div class="flex a-center j-center desc">
      <cardItem color="#E57471">
        <template #title>确诊</template>
        <template #num>{{allData.diagnosed}}</template>
        <template #compare>+{{allData.history[0].confirmedNum - allData.history[1].confirmedNum}}</template>
      </cardItem>
      <cardItem color="#DDA451">
        <template #title>疑似</template>
        <template #num>{{allData.suspect}}</template>
        <template #compare v-if="allData.history[0].suspectedNum > allData.history[1].suspectedNum">+{{allData.history[0].suspectedNum - allData.history[1].suspectedNum}}</template>
        <template #compare else>{{allData.history[0].suspectedNum - allData.history[1].suspectedNum}}</template>
      </cardItem>
      <cardItem color="#919399">
        <template #title>死亡</template>
        <template #num>{{allData.death}}</template>
        <template #compare>+{{allData.history[0].deathsNum - allData.history[1].deathsNum}}</template>
      </cardItem>
      <cardItem color="#7EBE50">
        <template #title>治愈</template>
        <template #num>{{allData.cured}}</template>
        <template #compare>+{{allData.history[0].curesNum - allData.history[1].curesNum}}</template>
      </cardItem>
    </div>
  </div>
</template>

<script>
import cardItem from "./CardItem";
export default {
  data() {
    return {};
  },
  components: {
    cardItem
  },
  props: {
    allData: {
      type: Object,
      default: () => {}
    }
  },
  methods: {},
  mounted() {
    // console.log(this.allData);
    console.log(this.allData.history[0].suspectedNum);
    console.log(this.allData.history[1].suspectedNum);

  },
  watch: {},
  computed: {},
  filters: {}
};
</script>

<style scoped lang='scss'>
.container {
  .top {
    width: 100%;
    text-align: center;
  }
  .desc {
    margin-top: 20px;
  }
}
</style>