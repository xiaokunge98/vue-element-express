<template>
  <!-- 外层大盒子 -->
  <div class="container">
    <head-top :router-path="$route.path"></head-top>
    <div id="mianContent">
      <div class="info_echarts">
        <my-echarts
          :p-width="400"
          :p-height="400"
          :p-options="options"
        ></my-echarts>
      </div>
      <div class="user_info">
        <user-info></user-info>
      </div>
    </div>
  </div>
</template>
<script>
import headTop from "../components/headTop/headTop.vue";
import myEcharts from "../components/echarts/myEcharts.vue";
import userInfo from "../components/userInfo/userInfo.vue";
import request from "../request/index";
export default {
  name: "home",
  data() {
    return {
      options: {
        title: {
          text: "人员信息"
        },
        tooltip: {},
        legend: {
          data: ["信息"]
        },
        xAxis: {
          data: ["注册人数", "访问次数", "访问时长", "年龄范围", "人员类型"]
        },
        yAxis: {},
        series: [
          {
            name: "信息",
            type: "bar",
            data: [8, 20, 36, 40, 10]
          },
          {
            type: "pie",
            center: ["80%", 65], //距离左边的位置65%，距离顶部的位置60，数值越大越远
            radius: 25, //pie圆形的大小
            data: [
              { name: "初中生", value: 80 },
              { name: "高中生", value: 10 },
              { name: "大学生", value: 10 }
            ]
          },
          {
            type: "line",
            name: "分布",
            data: [8, 20, 36, 40, 10]
          }
        ]
      },
      userList: []
    };
  },
  components: {
    headTop,
    myEcharts,
    userInfo
  },
  created() {
    // this.getUserList();
    // console.log(this.options);
  },
  methods: {
    async getUserList() {
      const { data } = await request.get("/api/users/allUsers");
      this.options.series[0].data.push(data.length);
      this.options.series[0].data.push(20);
      this.options.series[0].data.push(36);
      this.options.series[0].data.push(40);
      this.options.series[0].data.push(10);
      this.options.series[2].data = this.options.series[0].data;
    }
  }
};
</script>
<style scoped>
.container {
  box-sizing: border-box;
  overflow: hidden;
}
#mianContent {
  margin-top: 20px;
  width: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
}
.user_info,
.info_echarts {
  float: left;
  width: 50%;
  display: flex;
  justify-content: center;
}
</style>
