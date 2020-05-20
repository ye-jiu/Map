<template>
  <div class="map">
    <div id="chart" style="width: 80%;height:1800px;"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china'
import jsonp from 'jsonp'

let option ={
    title:{
        text:'地图',
        x:'center',
        textStyle:{
            fontSize: '60',
            color:'#9c0505',
        }
    },
    tooltip:{
        trigger:'item',
        formatter:'确诊：{c}',
    },
    series:[
        {
            type:'map',
            map:'china',
            // data:[
            //     {name:'北京', value:200},
            //     {name:'四川', value:2000},
            //     {name:'湖北', value:20000},
            // ],
            label:{
                show:true,
                color:'black',
                fontSize: '36',
            },
            zoom:1.2,
            itemStyle:{
                    borderColor:'black',
                },
            emphasis:{
                label:{
                    color:'#fff',
                    fontSize:'40',
                },
                itemStyle:{
                    areaColor:'red',
                }
            },
        }
    ],
    visualMap:{
        type:'piecewise',
        show:true,
        pieces:[
            {min:10000},
            {min:1000, max:9999},
            {min:100, max:999},
            {min:10, max:99},
            {min:1, max:9},
            {value:0},
        ],
        inRange:{
            color:['#fff','#ffaa85','#660208']
        },
        itemWidth:50,
        itemHeight:50,
        symbolSize:20
    }
};
export default {
  data(){
    return {
      myChart:''
    }
  },
  mounted(){  
    this.getData();
    this.myChart = echarts.init(document.getElementById('chart'));
    // this.myChart.setOption(option);
  },
  methods:{
      getData(){
          jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',(err,data)=>{
            //   console.log(data)
              var lists = data.data.list.map(item=>{return {name: item.name, value: item.value}})
              option.series[0].data = lists;
              this.myChart.setOption(option);
          })
      }
  }
}
</script>
<style scoped>
#chart {
  margin: 0 auto;
}
</style>