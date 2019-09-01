<template>
	<div class="ICmain">
		<div class="ICheader">
			<Row :gutter="16">
				<Col span="12">
					<div class="ICtitle">{{$t("Fault Devices")}}<lable class="ICLable" style="color: #da2b2b;font-weight: 600;">{{faultdevice}}</lable>
					</div>
				</Col>
				<Col span="12">
					<div class="ICtitle">{{$t("Offline Equipment")}}<lable class="ICLable" style="color:#ffc000;font-weight: 600;">{{alldevice-onlinedevice}}</lable>
					</div>
				</Col>
			</Row>
			<Row :gutter="16">
				<Col span="12">
					<div class="ICtitle">{{$t("Tasks Completed Today")}}<lable class="ICLable" style="color:#0080f2;font-weight: 600;">{{today}}</lable>
					</div>
				</Col>
				<Col span="12">
					<div class="ICtitle">{{$t("Online Devices")}}<lable class="ICLable" style="color:#00d484;font-weight: 600;">{{onlinedevice}}</lable>
					</div>
				</Col>
			</Row>
		</div>
		<div class="onlineChart" id="onlineChart" style="width: 100%;height: 130px;"></div>
		<!-- <v-chart class="status-pie" :options="statusPie" theme="asynciot" autoresize/> -->
		<section class="status-pie-name">
		  <p>在线</p>
		  <p>离线</p>
		  <p>异常</p>
		</section>
	</div>
</template>

<script>
	import echarts from 'echarts'
	import ECharts from 'vue-echarts';
	
	//import statusPie from '@/YDemo/Dashboard/status-pie'
	var dataStyle = {
	  normal: {
	    label: {
	        show: false
	    },
	    labelLine: {
	        show: false
	    },
	    shadowBlur: 0,
	    shadowColor: '#203665'
	  }
	};
	export default {
		components: {
			'v-chart': ECharts
		},
		data() {
			return {
				statusPie:{
					
				},
				aa:45,
				//allevents: 0,
				alldevice: 0,
				onlinedevice: 0,
				faultdevice: 0,
				today: 0,
			}
		},
		mounted() {
			
			this.getinfo();
			window.onresize= () => {
				this.charts.resize()
			}
		},
		methods: {
			async getinfo() {
				var res
				var ech
				this.faultdevice=0
				res = await this.$api.getRepair({
					search_info: '',page: 1,num: 4,isreg: "True",state:'treated',order_type:'',result:'',device_id:'',
					finish_starttime:Date.parse(this.$format(new Date(), 'YYYY-MM-DD')),
				})
				if (res.data.code == 0){
				this.today = res.data.data.totalNumber
				}
				res = await this.$api.getRepair({
					search_info: '',page: 1,num: 4,isreg: "True",state:'untreated',order_type:'',result:'',device_id:'',
				})
				if (res.data.code == 0){
				this.faultdevice =res.data.data.totalNumber
				}
				res = await this.$api.devices({page: 1,num: 10,isreg: '',item:'bd'})
				if (res.data.code == 0){
				this.alldevice =res.data.data.totalNumber
				}
				res = await this.$api.devices({page: 1,num: 10,isreg: '',state:'online',item:'bd'})
				if (res.data.code == 0){
				this.onlinedevice =res.data.data.totalNumber
				}
				this.drawPie();
			},
			drawPie() {
				this.charts = echarts.init(document.getElementById("onlineChart"))
				this.charts.setOption({
						tooltip: {
							trigger: 'item',
							formatter: " {c} ({d}%)"
						},
						series: [{
						  type: 'pie',
						  clockWise: false,
						  radius: [38, 44],
						  itemStyle: dataStyle,
						  hoverAnimation: false,
						  center: ['15%', '50%'],
						  data: [{
						    value: this.onlinedevice,
						    label: {
						      normal: {
						        rich: {
						          a: {
						            color: '#ffffff',
						            align: 'center',
						            fontSize: 20,
						            fontWeight: "bold"
						          },
						        },
						        formatter: function (params) {
						          return "{a|" + params.value + "}";
						        },
						        position: 'center',
						        show: true,
						        textStyle: {
						          fontSize: '14',
						          fontWeight: 'normal',
						          color: '#fff'
						        }
						      }
						    },
						    itemStyle: {
						      normal: {
						        color: '#07a3f3',
						        shadowColor: '#07a3f3',
						        shadowBlur: 0
						      }
						    }
						  }, {
						    value: this.alldevice-this.onlinedevice,
						    itemStyle: {
						      normal: {
						        color: '#152036'
						      },
						      emphasis: {
						        color: '#152036'
						      }
						    }
						  }]
						}, {
						  type: 'pie',
						  clockWise: false,
						  radius: [38, 44],
						  itemStyle: dataStyle,
						  hoverAnimation: false,
						  center: ['50%', '50%'],
						  data: [{
						    value: this.alldevice-this.onlinedevice,
						    label: {
						      normal: {
						        rich: {
						          a: {
						            color: '#ffffff',
						            align: 'center',
						            fontSize: 20,
						            fontWeight: "bold"
						          },
						        },
						        formatter: function (params) {
						          return "{a|" + params.value + "}";
						        },
						        position: 'center',
						        show: true,
						        textStyle: {
						          fontSize: '14',
						          fontWeight: 'normal',
						          color: '#fff'
						        }
						      }
						    },
						    itemStyle: {
						      normal: {
						        color: '#f2b028',
						        shadowColor: '#f2b028',
						        shadowBlur: 0
						      }
						    }
						  }, {
						    value: this.onlinedevice,
						    itemStyle: {
						      normal: {
						        color: '#152036'
						      },
						      emphasis: {
						        color: '#152036'
						      }
						    }
						  }]
						}, {
						  type: 'pie',
						  clockWise: false,
						  radius: [38, 44],
						  itemStyle: dataStyle,
						  hoverAnimation: false,
						  center: ['85%', '50%'],
						  data: [{
						    value: this.faultdevice,
						    label: {
						      normal: {
						        rich: {
						          a: {
						            color: '#ffffff',
						            align: 'center',
						            fontSize: 20,
						            fontWeight: "bold"
						          },
						        },
						        formatter: function (params) {
						          return "{a|" + params.value + "}";
						        },
						        position: 'center',
						        show: true,
						        textStyle: {
						          fontSize: '14',
						          fontWeight: 'normal',
						          color: '#fff'
						        }
						      }
						    },
						    itemStyle: {
						      normal: {
						        color: '#e15d68',
						        shadowColor: '#e15d68',
						        shadowBlur: 0
						      }
						    }
						  }, {
								name: 'invisible',
						    value: this.alldevice-this.faultdevice,
						    itemStyle: {
						      normal: {
						        color: '#152036'
						      },
						      emphasis: {
						        color: '#152036'
						      }
						    }
						  }]
						}]
					}

				)
			}

		},


	}
</script>

<style>
	.ICmain {
		height: 100%;
		width: 100%;
	}

	.ICheader {
		width: 100%;
		height: 120px;
		/* border: 1px solid #5ea8d9; */
		/* border-radius:10px ; */
	}

	.ICtitle {
		width: 160px;
		height: 50px;
		float: left;
		font-size: 15px;
		color: #ffffff;
	}

	.ICLable {
		font-size: 30px;
		margin-left: 20px;
	}
	.status-pie {
	  height: 130px;
		width: 100% !important;
	}
	.status-pie-name {
	  display: flex;
	  flex-flow: row nowrap;
	  justify-content: space-around;
	  align-content: center;
	  margin-bottom: 10px;
	  margin-left: -10px;
	  margin-right: -10px;
	  color: #a6c2d2;
	  p {
	    width: 100%;
	    text-align: center;
	  }
	}
</style>
