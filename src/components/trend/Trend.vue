<template>
  <div>
    <ve-line :data="chartData1" :colors="colors1"></ve-line>
    <ve-line :data="chartData2" :colors="colors2"></ve-line>
    <div class="title">
      全国
      <span class="active1">确诊人数</span>总览
    </div>
    <ve-pie :data="chartData3"></ve-pie>
    <div class="title">
      全国
      <span class="active2">疑似人数</span>总览
    </div>
    <ve-pie :data="chartData4"></ve-pie>
    <div class="title">
      全国
      <span class="active3">治愈人数</span>总览
    </div>
    <ve-pie :data="chartData5"></ve-pie>
    <div class="title">
      全国
      <span class="active4">死亡人数</span>总览
    </div>
    <ve-pie :data="chartData6"></ve-pie>
  </div>
</template>

<script>
export default {
  name: "trend",
  data() {
    return {
      //确诊和疑似
      chartData1: {
        columns: ["date", "确诊人数", "疑似人数"],
        rows: []
      },
      colors1: ["#e67a77", "#dda451"],
      //治愈和死亡
      chartData2: {
        columns: ["date", "治愈人数", "死亡人数"],
        rows: []
      },
      //确诊
      chartData3: {
        columns: ["name", "确诊人数"],
        rows: []
      },
      //疑似
      chartData4: {
        columns: ["name", "疑似人数"],
        rows: []
      },
      //治愈
      chartData5: {
        columns: ["name", "治愈人数"],
        rows: []
      },
      //死亡
      chartData6: {
        columns: ["name", "死亡人数"],
        rows: []
      },
      colors2: ["#8AC460", "#919399"]
    };
  },
  props: {
    allData: {
      type: Object,
      default: () => {}
    }
  },
  components: {},
  methods: {},
  mounted() {
    let arr1 = [];
    let arr2 = [];
    let arr3 = [];
    let arr4 = [];
    let arr5 = [];
    let arr6 = [];
    this.allData.history.reverse().map(item => {
      arr1.push({
        date: item.date,
        确诊人数: item.confirmedNum,
        疑似人数: item.suspectedNum
      });
      arr2.push({
        date: item.date,
        治愈人数: item.curesNum,
        死亡人数: item.deathsNum
      });
    });
    let num1 = 0;
    let num2 = 0;
    let num3 = 0;
    let num4 = 0;
    this.allData.area.map(item => {
      if (item.provinceShortName === "湖北") {
        arr3.push({
          name: item.provinceShortName,
          确诊人数: item.confirmedCount
        });
        arr3.push({
          name: item.cities[0].cityName,
          确诊人数: item.confirmedCount
        });
        arr4.push({
          name: item.provinceShortName,
          疑似人数: item.suspectedCount
        });
        arr4.push({
          name: item.cities[0].cityName,
          疑似人数: item.suspectedCount
        });
        arr5.push({
          name: item.provinceShortName,
          治愈人数: item.curedCount
        });
        arr5.push({
          name: item.cities[0].cityName,
          治愈人数: item.cities[0].curedCount
        });
        arr6.push({
          name: item.provinceShortName,
          死亡人数: item.deadCount
        });
        arr6.push({
          name: item.cities[0].cityName,
          死亡人数: item.cities[0].deadCount
        });
      } else {
        num1 += item.confirmedCount;
        num2 += item.suspectedCount;
        num3 += item.curedCount;
        num4 += item.deadCount;
      }
    });
    arr3.push({
      name: "非湖北",
      确诊人数: num1
    });
    arr4.push({
      name: "非湖北",
      疑似人数: num2
    });
    arr5.push({
      name: "非湖北",
      治愈人数: num3
    });
    arr6.push({
      name: "非湖北",
      死亡人数: num4
    });
    //确诊和疑似
    this.chartData1.rows = arr1;
    //治愈和死亡
    this.chartData2.rows = arr2;
    //确诊
    this.chartData3.rows = arr3;
    //疑似
    this.chartData4.rows = arr4;
    //治愈
    this.chartData5.rows = arr5;
    //死亡
    this.chartData6.rows = arr6;
    // console.log(this.chartData1);
  },
  watch: {},
  computed: {},
  filters: {}
};
</script>

<style scoped lang='scss'>
.title {
  margin: 20px 0 20px 20px;
  font-weight: 700;
  .active1 {
    color: #e67a77;
  }
  .active2 {
    color: #dda451;
  }
  .active3 {
    color: rgb(45, 252, 45);
  }
  .active4 {
    color: rgb(197, 201, 197);
  }
}
</style>