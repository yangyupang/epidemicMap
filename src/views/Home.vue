<template>
  <div class="home">
    <navHeader></navHeader>

    <a-tabs defaultActiveKey="1">
      <a-tab-pane tab="疫情地图" key="1">
        <detail :allData="allData" v-if="allData && allData.history"></detail>
        <navMap :allData="allData" v-if="allData && allData.area"></navMap>
        <table-data :allData="allData" v-if="allData && allData.area"></table-data>
      </a-tab-pane>
      <a-tab-pane tab="最新消息" key="2" forceRender>
        <news></news>
      </a-tab-pane>
      <a-tab-pane tab="辟谣消息" key="3">
        <rumour></rumour>
      </a-tab-pane>
      <a-tab-pane tab="疫情趋势" key="4">
        <trend :allData="allData" v-if="allData && allData.history"></trend>
      </a-tab-pane>
    </a-tabs>

    <div class="footer flex a-center j-between">
      <div>中国加油 武汉加油</div>
      <div @click="git()">github</div>
    </div>
  </div>
</template>

<script>
import api from "../http/api";
import navHeader from "../components/navHeader/NavHeader";
import navMap from "../components/navMap/NavMap";
import detail from "../components/detail/Detail";
import tableData from "../components/tableData/TableData";
import news from "../components/news/News";
import rumour from "../components/rumour/Rumour";
import trend from "../components/trend/Trend";
export default {
  data() {
    return {
      allData: {}
    };
  },
  components: {
    navHeader,
    detail,
    navMap,
    tableData,
    news,
    rumour,
    trend
  },
  methods: {
    getData() {
      api.getData().then(res => {
        if (res.errcode === 0) {
          this.allData = res.data;
          console.log(this.allData);
        }
      });
    },
    git() {
      window.open("https://github.com/yangyupang/epidemicMap");
    }
  },
  mounted() {
    this.getData();
  },
  watch: {},
  computed: {},
  filters: {}
};
</script>

<style scoped lang='scss'>
.footer {
  width: 100%;
  padding: 0 20px;
  margin-bottom: 15px;
  color: skyblue;
}
</style>