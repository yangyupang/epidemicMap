<template>
  <div class="container">
    <div class="content" v-for="(item, index) in list" :key="index">
      <a-card>
        <div class="img">
          <img :src="item.imgsrc" />
          <div class="text flex a-center j-center">
            <div>{{item.explain}}</div>
            <a-icon type="close" class="icon" />
          </div>
        </div>
        <div class="title">{{item.title}}</div>
        <div class="desc">{{item.desc}}</div>
      </a-card>
    </div>
  </div>
</template>

<script>
import api from '../../http/api'
export default {
  data() {
    return {
      list: []
    };
  },
  components: {},
  methods: {
    getData() {
      api.getRumour().then(res => {
        if (res.code === 200) {
          this.list = res.newslist;
        }
        // console.log(res);
      });
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
.content {
  .img {
    margin-bottom: 20px;
    position: relative;
    img {
      width: 100%;
      height: 100%;
      border-radius: 24px 24px 0 0;
    }
    .text {
      position: absolute;
      width: 140px;
      height: 140px;
      border-radius: 50%;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      border: 3px solid red;
      font-size: 28px;
      color: red;
      font-weight: 700;
      div {
        transform: skew(-40deg);
      }
      .icon {
        font-size: 132px;
        color: #fff;
        position: absolute;
        top: 0;
        left: 2px;
        z-index: -1;
      }
    }
  }
  .title {
    font-size: 16px;
    font-weight: 700;
  }
}
</style>