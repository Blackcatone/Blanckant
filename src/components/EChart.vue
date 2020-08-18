<template>
  <div style="height:100%" ref="echart">echart</div>
</template>

<script type="text/javascript">
import echarts from "echarts";
export default {
  props: {
    chartData: {
      type: Object,
      default() {
        return {
          xData: [],
          series: []
        };
      }
    },
    isAxisChart: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    options() {
      return this.isAxisChart ? this.axisOption : this.normalOption;
    },
    isCollapse() {
      return this.$store.state.HanderTab.isCollapse;
    }
  },
  watch: {
    chartData: {
      handler: function() {
        this.initChart();
      },
      deep: true
    },
    isCollapse() {
      setTimeout(() => {
        this.resizeChart();
      }, 300);
    }
  },
  data() {
    return {
      echart: null,
      axisOption: {
        legend: {
          textStyle: {
            color: "#535c68"
          }
        },
        tooltip: {
          trigger: "axis"
        },
        xAxis: {
          type: "category",
          data: [],
          axisLine: {
            lineStyle: { color: "#686de0" }
          },
          axisLabel: {
            color: "#535c68"
          }
        },
        yAxis: [
          {
            type: "value",
            axisLine: {
              lineStyle: { color: "#686de0" }
            }
          }
        ],
        series: [],
        color: [
          "#4962FC",
          "#ff7979",
          "#dd3ee5",
          "#12e78c",
          "#fe8104",
          "#01C2F9",
          "#F4CB29",
          "#FD9E06"
        ]
      },
      normalOption: {
        tooltip: {
          trigger: "item"
        },
        color: [
          "#f6e58d",
          "#ffbe76",
          "#ff7979",
          "#badc58",
          "#dff9fb",
          "#22a6b3"
        ],
        series: []
      }
    };
  },
  methods: {
    initChart() {
      this.initChartData();
      if (this.echart) {
        this.echart.setOption(this.options);
      } else {
        this.echart = echarts.init(this.$refs.echart);
        this.echart.setOption(this.options);
      }
    },
    initChartData() {
      if (this.isAxisChart) {
        this.axisOption.xAxis.data = this.chartData.xData;
        this.axisOption.series = this.chartData.series;
      } else {
        this.normalOption.series = this.chartData.series;
      }
    },
    resizeChart() {
      this.echart ? this.echart.resize() : "";
    }
  },
  mounted() {
    window.addEventListener("resize", this.resizeChart);
  },
  destroyed() {
    window.removeEventListener("resize", this.resizeChart);
  }
};
</script>

<style lang="scss" scoped>
</style>
