<template>
  <div class="container">
    <div class="top">各省最新疫情查询(点击选择具体省份)</div>
    <div class="select">
      <a-select @change="changeSelect" defaultValue="全国" style="width:100%">
        <a-select-option v-for="(item, index) in provinces" :key="index" :value="item">{{item}}</a-select-option>
      </a-select>
    </div>
    <div class="charts">
      <ve-map
        ref="charts"
        :settings="chartSettings"
        :legend-visible="false"
        :data="mapData"
        :grid="grid"
        :visual-map="visualMap"
      ></ve-map>
    </div>
  </div>
</template>

<script>
import "echarts/lib/component/visualMap";
import pinyin from "js-pinyin";
pinyin.setOptions({ checkPolyphone: false, charCase: 0 });
export default {
  name: "navMap",
  data() {
    return {
      charts: null,
      chartSettings: {
        position: "china"
      },
      visualMap: [
        {
          type: "piecewise",
          splitNumber: 5,
          pieces: [
            { min: 1000, label: ">1000" },
            { min: 500, max: 999, label: "500-999" },
            { min: 100, max: 499, label: "100-499" },
            { min: 10, max: 99, label: "10-99" },
            { min: 1, max: 9, label: "1-9" }
          ],
          top: "top",
          left: "center",
          right: "auto",
          bottom: "auto",
          orient: "horizontal"
        }
      ],
      grid: {
        top: 0
      },
      mapData: {
        columns: ["name", "确诊"],
        rows: []
      }
    };
  },
  props: {
    allData: {
      type: Object,
      default: () => {}
    }
  },
  components: {},
  methods: {
    //改变 搜索框中的值
    changeSelect(val) {
      if (val !== "全国") {
        this.chartSettings.position =
          "province/" + pinyin.getFullChars(val).toLowerCase();
        let area = this.allData.area;
        let province = area.filter(item => item.provinceShortName === val)[0];
        let arr = [];
        province.cities.map(item => {
          arr.push({
            name: item.cityName + "市",
            确诊: item.confirmedCount
          });
        });
        this.mapData.rows = arr;
      } else {
        this.chartSettings.position = "china";
        let arr = [];
        this.allData.area.map(item => {
          arr.push({
            name: item.provinceShortName,
            确诊: item.confirmedCount
          });
          this.mapData.rows = arr;
        });
      }
    }
  },
  mounted() {
    let arr = [];
    this.allData.area.map(item => {
      arr.push({
        name: item.provinceShortName,
        确诊: item.confirmedCount
      });
      this.mapData.rows = arr;
    });
    // this.$nextTick(_ => {
    //   this.charts.echarts.resize();
    // });
    // window.onresize = () => {
    //   this.echarts.resize();
    // };
  },
  watch: {},
  computed: {
    provinces(item) {
      let arr = [];
      arr.unshift("全国");
      this.allData.area.map(item => {
        arr.push(item.provinceShortName);
      });
      return arr;
    }
  },
  filters: {}
};
</script>

<style scoped lang='scss'>
.container {
  .top {
    width: 100%;
    text-align: center;
    margin: 20px 0 10px 0;
  }
  .select {
    width: 90%;
    margin-left: 5%;
    margin-bottom: 20px;
  }
  .charts {
    width: 100%;
    height: 400px;
  }
}
</style>