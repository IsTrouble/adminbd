<template >
	<div id="main" style="width:100%;height: 383px;"></div>
	<!-- <v-chart class="element" :options="element" theme="asynciot" autoresize/> -->
</template>

<script>
	import echarts from 'echarts'
	import ECharts from 'vue-echarts';
	
	//import element from '@/YDemo/Dashboard/element'
    export default {
		components: {
			//'v-chart': ECharts
		},
        name: '',
        data () {
            return {
				topcode:[],
				codelist:['维护','过流','母线过压','母线欠压','输入缺相',
						'输出缺相','输出过力矩','编码器故障','模块过热','运行接触器故障',
						'抱闸接触器故障','封星继电器故障','抱闸开关故障','运行中安全回路断开','运行中门锁断开',
						'门锁短接故障','层站召唤通讯故障','轿厢通讯故障','并联通讯故障','开门故障',
						'关门故障','开关门到位故障','平层信号异常','终端减速开关故障','下限位信号异常',
						'上限位信号异常','打滑故障','电梯速度异常','电机反转故障','磁极位置学习故障',
						'E30','停车速度检测','井道自学习故障','马达过热故障','制动力严重不足',
						  '制动力不足警告',]
			
            }
        },
        methods:{
			refresh(){
			this.drawPie('main');
			setTimeout(() => {
				this.refresh();
			},1000)
			},
			async getfaultfreq(){
				// var time=Date.parse(this.$format(new Date(), 'yyyy-MM-DD'))
				let res = await this.$api.faultfreq({item:'bd'})
				if (res.data.code == 0){
					this.topcode=res.data.list
					for (var i=6;i<this.topcode.length;i++){
						this.topcode[5].counter=this.topcode[5].counter+this.topcode[i].counter
					}
				}
				this.refresh()
			},
            drawPie(id){
               this.charts = echarts.init(document.getElementById(id))
			   this.charts.resize()
               this.charts.setOption(
			   {
                tooltip: {
                  trigger: 'item',
                  formatter: "{b} : {c} ({d}%)"
                },
                visualMap: {
                  show: false,
                  min: 500,
                  max: 600,
                  inRange: {
                    //colorLightness: [0, 1]
                  }
                },
                series: [{
					name:'故障原因',
                  type: 'pie',
                  radius: '50%',
                  center: ['50%', '50%'],
                  color: ['rgb(131,249,103)', '#FBFE27', '#FE5050', '#1DB7E5','#8669FF','#FFCC20'], //'#FBFE27','rgb(11,228,96)','#FE5050'
                  data: [{value:this.topcode[0].counter, name: this.$t('E'+this.topcode[0].code.toString(16))},
							{value:this.topcode[1].counter, name: this.$t('E'+this.topcode[1].code.toString(16))},
							{value:this.topcode[2].counter, name: this.$t('E'+this.topcode[2].code.toString(16))},
							{value:this.topcode[3].counter, name: this.$t('E'+this.topcode[3].code.toString(16))},
							{value:this.topcode[4].counter, name: this.$t('E'+this.topcode[4].code.toString(16))},
						{value:this.topcode[5].counter, name: this.$t('Other')}
                  ].sort(function (a, b) {
                    return a.value - b.value
                  }),
                  roseType: 'radius',
                
                  label: {
                    normal: {
                      formatter: ['{c|{c}}', '{b|{b}}'].join('\n'),
                      rich: {
                        c: {
                          color: 'rgb(241,246,104)',
                          fontSize: 20,
                          fontWeight: 'bold',
                          lineHeight: 5
                        },
                        b: {
                          color: 'rgb(98,137,169)',
                          fontSize: 12,
                          height: 40
                        },
                      },
                    }
                  },
                  labelLine: {
                    normal: {
                      lineStyle: {
                        color: 'rgb(98,137,169)',
                      },
                      smooth: 0.2,
                      length: 10,
                      length2: 20,
                
                    }
                  },
                  itemStyle: {
                    normal: {
                      shadowColor: 'rgba(0, 0, 0, 0.8)',
                      shadowBlur: 50,
                    }
                  }
                }]
               })
            }
        },
      //调用
	  created() {
	  	this.getfaultfreq({item:'bd'});
	  },
	}

</script>

<style lang="scss" scoped>
	.element {
	  height: 383px;
	  width: 100% !important;
	}
</style>
