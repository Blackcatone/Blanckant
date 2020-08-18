<template>
  <el-row class="home" :gutter="20">
    <el-col :span="8">
      <el-card shadow="hover">
        <div class="user">
          <img :src="userImg" />
          <div class="userinfo">
            <p class="name">Pink</p>
            <p class="access">超级管理员</p>
          </div>
        </div>
        <div class="lgoin-info">
          <p>
            上次登录时间:
            <span>2020-03-24</span>
          </p>
          <p>
            上次登录地点:
            <span>成都</span>
          </p>
        </div>
      </el-card>
      <el-card shadow="hover" style="height:522px; margin-top:20px;">
        <el-table :data="tableData">
          <el-table-column
            show-overflow-tooltip
            v-for="(val, key) in tableLable"
            :key="key"
            :prop="key"
            :label="val"
          ></el-table-column>
        </el-table>
      </el-card>
    </el-col>
    <el-col :span="16">
      <div class="num">
        <el-card
          shadow="hover"
          v-for="item in countData"
          :key="item.name"
          :body-style="{ display: 'flex', padding: 0 }"
        >
          <i class="icon" :class=" `el-icon-${item.icon}` " :style="{ background: item.color }"></i>
          <div class="tedail">
            <p class="num">￥ {{ item.value }}</p>
            <p class="text">{{ item.name }}</p>
          </div>
        </el-card>
      </div>
      <el-card shadow="hover">
        <echart style="height:280px" :chartData="echartData.order"></echart>
      </el-card>
      <div class="graph">
        <el-card shadow="hover">
          <echart style="height:260px" :chartData="echartData.user"></echart>
        </el-card>
        <el-card shadow="hover">
          <echart style="height:260px" :chartData="echartData.video" :isAxisChart="false"></echart>
        </el-card>
      </div>
    </el-col>
  </el-row>
</template>

<script type="text/javascript">
import Echart from "../../components/EChart";
export default {
  components: {
    Echart
  },
  created() {
    this.getHomeData();
  },
  data() {
    return {
      userImg: require("@/assets/logo.png"),
      countData: [
        {
          name: "今日支付订单",
          value: 648,
          icon: "success",
          color: "#6ab04c"
        },
        {
          name: "今日收藏订单",
          value: 128,
          icon: "star-on",
          color: "#f9ca24"
        },
        {
          name: "今日未支付订单",
          value: 1028,
          icon: "s-goods",
          color: "#eb4d4b"
        },
        {
          name: "本月支付订单",
          value: 2648,
          icon: "success",
          color: "#6ab04c"
        },
        {
          name: "本月收藏订单",
          value: 1080,
          icon: "star-on",
          color: "#f9ca24"
        },
        {
          name: "本月未支付订单",
          value: 980,
          icon: "s-goods",
          color: "#eb4d4b"
        }
      ],
      tableData: [],
      tableLable: {
        name: "课程",
        todayBuy: "今日购买",
        monthBuy: "本月购买",
        totalBuy: "总购买"
      },
      echartData: {
        order: {
          xData: [],
          series: []
        },
        user: {
          xData: [],
          series: []
        },
        video: {
          series: []
        }
      }
    };
  },
  methods: {
    getHomeData() {
      this.$http.get("/home/getData").then(res => {
        res = res.data;
        this.tableData = res.data.tableData;
        // 订单折线图数据
        const order = res.data.orderData;
        this.echartData.order.xData = order.date;
        // 取出series中的键名
        let keyArray = Object.keys(order.data[0]);
        keyArray.forEach(key => {
          this.echartData.order.series.push({
            name: key === "wechat" ? "小程序" : key,
            data: order.data.map(item => item[key]),
            type: "line"
          });
        });
        // 柱状图
        this.echartData.user.xData = res.data.userData.map(item => item.date);
        this.echartData.user.series.push({
          name: "新增用户",
          data: res.data.userData.map(item => item.new),
          type: "bar"
        });
        this.echartData.user.series.push({
          name: "活跃用户",
          data: res.data.userData.map(item => item.active),
          type: "bar",
          barGap: "0"
        });
        // 饼状图
        this.echartData.video.series.push({
          data: res.data.videoData,
          type: "pie"
        });
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "@/assets/scss/home.scss";
</style>
