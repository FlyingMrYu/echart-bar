<template>
  <div class="bar-chart-content">
    <div :id="'chartbarBox'+id" :style="{width: width,height: height}"></div>
  </div>
</template>
<script>
import echarts from "echarts";
export default {
  name: "pieChart",
  mounted() {},
  props: ["id", "height", "option", "width", "optionsBar"],
  watch: {
    optionsBar(a) {
      this.init();
    }
  },
  methods: {
    init() {
      this.chartLine = echarts.init(
        document.getElementById(`chartbarBox${this.id}`)
      );
      // 滚动条显示位置
      let start = 0;
      if (
        this.optionsBar &&
        this.optionsBar[2] &&
        this.optionsBar[2].length != 0
      ) {
        let leinl = this.optionsBar[2][0].data.length;
        if (leinl > 10 && leinl < 1000) {
          start = 100 - parseInt((10 / leinl) * 100);
        } else if (leinl >= 1000) {
          start = 98;
        }
        console.log(leinl, "leinlleinl");
      }
      console.log(start, "start");
      var option;
      // 指定图表的配置项和数据
      option = {
        title: {
          show: false
        },
        toolbox: {
          top:0,
          left:20,
          feature: {
            saveAsImage: {}
          }
        },
        legend: {
          right: "7%",
          icon: "roundRect",
          itemHeight: 8, // 设置高度
          data: this.optionsBar[0]
          //  data: ["启动", "待机", "关机"]
        },
        color: ["#53bcff", "#ffca59", "#abb6c4"],
        tooltip: {
          //过滤掉统计的series
          trigger: "axis",
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: "shadow" // 默认为直线，可选为：'line' | 'shadow'
          }
          // formatter: function(params) {
          //   var res = params[0].name + "<br/>";
          //   for (let i = 0; i < params.length - 1; i++) {
          //     res += params[i].seriesName + ":" + params[i].value + "<br/>";
          //   }
          //   return res;
          // }
        },
        grid: {
          top: "20px",
          left: "0px",
          right: "7%",
          bottom: "18%",
          containLabel: true
        },
        xAxis: [
          {
            axisLine: { show: false },
            axisTick: { show: false },
            splitLine: { show: false },
            type: "category",
            data: this.optionsBar[1]
            // data: [
            //     "03-01",
            //     "03-02",
            //     "03-03",
            //     "03-04",
            //     "03-05",
            //     "03-06",
            //     "03-08"
            //   ]
          }
        ],
        yAxis: [
          {
            show: false,
            type: "value",
            axisLine: { show: false },
            axisTick: { show: false },
            splitLine: { show: false }
          }
        ],
        dataZoom: [
          {
            type: "slider",
            xAxisIndex: 0,
            bottom: "5%",
            showDetail: false,
            filterMode: "empty",
            start: start,
            end: 100
          },
          {
            type: "inside",
            show: true
          }
        ],
        series: this.optionsBar[2]
        //  series: [
        //     {
        //       name: "启动",
        //       type: "bar",
        //       barWidth: 35,
        //       stack: "搜索引擎",
        //       data: [620, 732, 701, 734, 1090, 1130, 1120]
        //     },
        //     {
        //       name: "待机",
        //       type: "bar",
        //       stack: "搜索引擎",
        //       data: [120, 132, 101, 134, 290, 230, 220]
        //     },
        //     {
        //       name: "关机",
        //       type: "bar",
        //       stack: "搜索引擎",
        //       data: [60, 72, 71, 74, 190, 130, 110]
        //     }
        //   ]
      };
      // 使用刚指定的配置项和数据显示图表。
      this.chartLine.setOption(option);
      var _that = this;
      window.onresize = function() {
        console.log(this.chartLine, " this.chartLine");
        _that.chartLine.resize();
        //myChart1.resize();    //若有多个图表变动，可多写
      };
    }
  }
};
</script>

<style scoped lang="scss">
.bar-chart-content {
}
</style>
