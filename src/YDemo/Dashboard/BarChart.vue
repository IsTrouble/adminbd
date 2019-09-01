<template>
	 <div>
		 <div id="BarChartMain" class="BarChartMain" style="width: 100%;height:310px;top: -40px;position: relative;margin-top: 40px;"></div>
	 </div>
</template>

<script>
	import echarts from 'echarts'
	export default{
		data(){
			return{
				LastWeek:'',
				LastWeekend:'',
				NowWeek:'',
				NowWeekend:'',
				Lastlist:{
					monday:'0',
					tuesday:'0',
					wensday:'0',
					thursday:'0',
					friday:'0',
					saturday:"0",
					sunday:"0",
				},
				list:{
					monday:'0',
					tuesday:'0',
					wensday:'0',
					thursday:'0',
					friday:'0',
					saturday:"0",
					sunday:"0",
				},
				topdevice:[],
				starttime:'',
				endtime:'',
			}
		},
		created() {
			this.getactivedoor();
		},
		methods:{
			refresh(){
				this.OrderCharts()
				setTimeout(() => {
					this.refresh();
				},1000)
			},
			async getactivedoor(){
				let res = await this.$api.activedoor()
				this.topdevice=res.data.list
				if (res.data.code == 0){
					this.topdevice=res.data.list
				}
				this.refresh()
			},
				OrderCharts() {
			
							let activedoor = this.$echarts.init(document.getElementById('BarChartMain'))
							activedoor.resize()
							activedoor.setOption({
								// triggerEvent:false,
								title: {
										show:false,
										text: '设备使用次数TOP5',
										textStyle: {
							              color: '#ffffff',
							              fontSize:'16'
							              },
								},
								xAxis: {
									type: 'category',
									data: [this.topdevice[0].device_name, 
											this.topdevice[1].device_name, 
											this.topdevice[2].device_name, 
											this.topdevice[3].device_name, 
											this.topdevice[4].device_name, 
									],
									// triggerEvent:false,
									splitLine: {
				               			show: false, 
					                    //  改变轴线颜色
					                    lineStyle: {
					                        // 使用深浅的间隔色
					                        color: ['white']
										}                            
									},
					                    //  改变x轴颜色
				                    axisLine:{
				                        lineStyle:{
				                             color: '#4d4d4d',
				                            width:0.5,//这里是为了突出显示加上的，可以去掉
				                        }
				                    },                         
				                    //  改变x轴字体颜色和大小
				                    axisLabel: {
				                        textStyle: {
				                              color: '#ffffff',
				                            fontSize:12
				                        },
										interval: 0,
										formatter:function(value)
										{
			
											var ret = "";//拼接加\n返回的类目项
											var maxLength = 5;//每项显示文字个数
											var valLength = value.length;//X轴类目项的文字个数
											var rowN = Math.ceil(valLength / maxLength); //类目项需要换行的行数
											if (rowN > 1)//如果类目项的文字大于3,
											{
												for (var i = 0; i < rowN; i++) {
													var temp = "";//每次截取的字符串
													var start = i * maxLength;//开始截取的位置
													var end = start + maxLength;//结束截取的位置
													//这里也可以加一个是否是最后一行的判断，但是不加也没有影响，那就不加吧
													temp = value.substring(start, end) + "\n";
													ret += temp; //凭借最终的字符串
												}
												return ret;
											}
											else {
												return value;
											}
										}
			
				                    },
								},
								yAxis: {
									type: 'value',
									splitLine: {
				               			show: false, 
				                    //  改变轴线颜色
				                    lineStyle: {
				                        // 使用深浅的间隔色
				                        color: ['white']
				                        }                            
				                    },
									// triggerEvent:false,
				                    //  改变x轴颜色
				                    axisLine:{
				                        barStyle:{
				                           color: '#4d4d4d'
				//                            width:8,//这里是为了突出显示加上的，可以去掉
				                        }
				                    },                         
				                    //  改变x轴字体颜色和大小
				                    axisLabel: {
				                        textStyle: {
				                            color: '#ffffff',
				                            fontSize: 10
				                        },
				                    },
								},
								series: [{
										data: [this.topdevice[0].counter,
												this.topdevice[1].counter,
												this.topdevice[2].counter,
												this.topdevice[3].counter,
												this.topdevice[4].counter,
										],
										// triggerEvent:false,
										type: 'bar',
										color:'#FF7F00',
										barWidth: 40,
										itemStyle: {
											 normal: {
											  color: {
											    type: 'linear',
											    x: 0,
											    y: 0,
											    x2: 0,
											    y2: 1,
											    colorStops: [{
											      offset: 0,
											      color: '#00d386' // 0% 处的颜色
											    }, {
											      offset: 1,
											      color: '#0076fc' // 100% 处的颜色
											    }],
											    globalCoord: false // 缺省为 false
											  },
											  barBorderRadius: [3, 3, 0, 0],
											}
										}
								}]
							})
							if(activedoor._$handlers.click){
								activedoor._$handlers.click.length = 0;
							}
							var _this=this
						},
		}
	}
</script>

<style>

</style>
