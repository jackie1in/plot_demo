<template>
  <div id="container"></div>
</template>
<script lang="ts">
import { Pie, Liquid, Gauge,Treemap, Radar } from '@antv/g2plot';

export default {
  name: 'Monitor',
  components: {},
  props: {
    moduleId: String,
  },
  data: () => {
    return {
      jsonData: []
    }
  },
  created() {

  },
  mounted() {
        // let data = this.getData(50000);
        // console.log(data)
        // const line = new Line('container', {
        //   data,
        //   padding: 'auto',
        //   xField: 'x',
        //   yField: 'y',
        //   xAxis: {
        //     type: 'time',
        //     mask: 'HH:mm:ss',
        //   },
        // });

        // line.render();
      // 水波图
      // const liquidPlot = new Liquid('container', {
      //   percent: 0.25,
      // });
      // liquidPlot.render();
      // 饼图
      const data = [
        { type: '分类一', value: 27 },
        { type: '分类二', value: 25 },
        { type: '分类三', value: 18 },
        { type: '分类四', value: 15 },
        { type: '分类五', value: 10 },
        { type: '其他', value: 5 },
      ];

      const piePlot = new Pie('container', {
        data,
        angleField: 'value',
        colorField: 'type',
      });

      piePlot.render();
      // 仪表盘
      // const gauge = new Gauge('container', {
      //   percent: 0.75,
      //   range: {
      //     color: '#5B8FF9',
      //   },
      //   statistic: {
      //     content: {
      //       formatter: ({ percent }) => `Rate: ${(percent * 100).toFixed(0)}%`,
      //     },
      //   },
      // });

      // gauge.render();
     // Treemap

    // const data = {
    //   name: 'root',
    //   children: [
    //     { name: '分类 1', value: 560 },
    //     { name: '分类 2', value: 500 },
    //     { name: '分类 3', value: 150 },
    //     { name: '分类 4', value: 140 },
    //     { name: '分类 5', value: 115 },
    //     { name: '分类 6', value: 95 },
    //     { name: '分类 7', value: 90 },
    //     { name: '分类 8', value: 75 },
    //     { name: '分类 9', value: 98 },
    //     { name: '分类 10', value: 60 },
    //     { name: '分类 11', value: 45 },
    //     { name: '分类 12', value: 40 },
    //     { name: '分类 13', value: 40 },
    //     { name: '分类 14', value: 35 },
    //     { name: '分类 15', value: 40 },
    //     { name: '分类 16', value: 40 },
    //     { name: '分类 17', value: 40 },
    //     { name: '分类 18', value: 30 },
    //     { name: '分类 19', value: 28 },
    //     { name: '分类 20', value: 16 },
    //   ],
    // };

    // const treemapPlot = new Treemap('container', {
    //   data,
    //   colorField: 'name',
    // });

    // treemapPlot.render();
    // 雷达图
    // const data = [
    //   { name: 'G2', star: 10371 },
    //   { name: 'G6', star: 7380 },
    //   { name: 'F2', star: 7414 },
    //   { name: 'L7', star: 2140 },
    //   { name: 'X6', star: 660 },
    //   { name: 'AVA', star: 885 },
    //   { name: 'G2Plot', star: 1626 },
    // ];
    // const radarPlot = new Radar('container', {
    //   data: data.map((d) => ({ ...d, star: Math.sqrt(d.star) })),
    //   xField: 'name',
    //   yField: 'star',
    //   meta: {
    //     star: {
    //       min: 0,
    //       nice: true,
    //     },
    //   },
    //   area: {},
    // });
    // radarPlot.render();
  },
  methods: {
    getData: (count: number) => {
      // generate an array of random data
      const data = [];
      const time = new Date().getTime();
      let c = 1;
      for (let i = 0; i < count; i += 1) {
        c++
        data.push({
          x: time + i * 1000,
          y: c + Math.pow(-1, i) * Math.random() * 1,
        });
      }
      return data;
    }
  },
  round2: (val: number) => {
    return Math.round(val * 100) / 100;
  },

  round3: (val: number) => {
    return Math.round(val * 1000) / 1000;
  },

  prepData: (packed: Array<number>) => {
    console.time("prep");

    // epoch,idl,recv,send,read,writ,used,free

    const numFields = packed[0];

    packed = packed.slice(numFields + 1);

    // 55,550 data points x 3 series = 166,650
    let data = [
      Array(packed.length / numFields),
      Array(packed.length / numFields),
      Array(packed.length / numFields),
      Array(packed.length / numFields),
    ];

    for (let i = 0, j = 0; i < packed.length; i += numFields, j++) {
      data[0][j] = packed[i] * 60;	// * 1e3
      data[1][j] = this.round3(100 - packed[i + 1]);
      data[2][j] = this.round2(100 * packed[i + 5] / (packed[i + 5] + packed[i + 6]));
      data[3][j] = packed[i + 3];
    }
    console.timeEnd("prep");
    return data;
  },
}
</script>
<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
