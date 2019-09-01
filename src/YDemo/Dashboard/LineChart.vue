<template>
	<div class="LineChartMain" id="LineChartMain" style="width: 100%;height:280px;position: relative;margin-top: 40px;"></div>
</template>

<script>
	import echarts from 'echarts'
	export default {
		data() {
			return {
				LastWeek: '',
				LastWeekend: '',
				NowWeek: '',
				NowWeekend: '',
				Lastlist: {
					monday: '0',
					tuesday: '0',
					wensday: '0',
					thursday: '0',
					friday: '0',
					saturday: "0",
					sunday: "0",
				},
				list: {
					monday: '0',
					tuesday: '0',
					wensday: '0',
					thursday: '0',
					friday: '0',
					saturday: "0",
					sunday: "0",
				},
			}
		},
		created() {
			this.screenheight = document.documentElement.clientHeight;
			this.LastWeek = this.getWeek(7)
			this.LastWeekend = this.getWeek(1)
			this.NowWeek = this.getWeek(0)
			this.NowWeekend = this.getWeek(-6)
			this.getLastData(this.LastWeek, this.LastWeekend)
			this.getData(this.NowWeek, this.NowWeekend)
			setTimeout(() => {
				this.refresh();
			}, 500)
		},
		methods: {
			refresh() {
				this.OrderCharts();
				setTimeout(() => {
					this.refresh();
				}, 2000)
			},
			async getLastData(val, item) {
				let res = await this.$api.orderCount({
					starttime: val,
					endtime: item,
				})
				this.Lastlist.monday = res.data.data.monday
				this.Lastlist.tuesday = res.data.data.tuesday
				this.Lastlist.wensday = res.data.data.wensday
				this.Lastlist.thursday = res.data.data.thursday
				this.Lastlist.friday = res.data.data.friday
				this.Lastlist.saturday = res.data.data.saturday
				this.Lastlist.sunday = res.data.data.sunday
			},
			async getData(val, item) {
				let res = await this.$api.orderCount({
					starttime: val,
					endtime: item,
				})
				this.list.monday = res.data.data.monday
				this.list.tuesday = res.data.data.tuesday
				this.list.wensday = res.data.data.wensday
				this.list.thursday = res.data.data.thursday
				this.list.friday = res.data.data.friday
				this.list.saturday = res.data.data.saturday
				this.list.sunday = res.data.data.sunday
			},
			getWeek(n) {
				var now = new Date()
				var year = now.getFullYear()
				var month = now.getMonth() + 1
				var date = now.getDate()
				var day = now.getDay()
				if (day !== 0) {
					n = n + day - 1
				} else {
					n = n + day
				}
				if (day) {
					if (month > 1) {
						month = month
					} else {
						year = year - 1
						month = 12
					}
				}
				now.setDate(now.getDate() - n);
				year = now.getFullYear();
				month = now.getMonth() + 1;
				date = now.getDate();
				var s = year + "-" + (month < 10 ? ('0' + month) : month) + "-" + (date < 10 ? ('0' + date) : date);
				return s
			},
			OrderCharts() {
				let test1 = this.$echarts.init(document.getElementById('LineChartMain'))
				test1.resize()
				test1.setOption({

					tooltip: {
						trigger: 'axis'
					},
					legend: {
						data: [this.$t('this week'), this.$t('last week')],
						textStyle: {
							color: '#ffffff',
							fontSize: 12
						},
					},
					xAxis: {
						type: 'category',
						boundaryGap: false,
						data: [this.$t('Mon'), this.$t('Tue'), this.$t('Wed'), this.$t('Thu'), this.$t('Fri'), this.$t('Sat'), this.$t(
							'Sun')],
						axisLabel: {
							textStyle: {
								color: '#ffffff',
								fontSize: 14
							},
						},
						axisLine: {show:false},
					},
					yAxis: {
						show:false,
						type: 'value',
						axisLabel: {
							textStyle: {
								color: '#ffffff',
								fontSize: 14
							},
						},
						 splitLine: {
							show: false
						},
						axisLine: {show:false},
						axisTick: {show:false},
						
					},
					series: [{
							name: this.$t('this week'),
							type: 'line',
							data: [this.list.monday, this.list.thursday, this.list.wensday, this.list.thursday,
								this.list.friday, this.list.saturday, this.list.sunday,
							],
							markPoint: {
								data: [{
										type: 'max',
										name: this.$t('max')
									},
									{
										type: 'min',
										name: this.$t('min')
									}
								]
							},
							itemStyle: {
								normal: {
									color: '#ff007e',
									lineStyle: {
										color: '#9320ff'
									}
								}
							},
						},
						{
							name: this.$t('last week'),
							type: 'line',
							data: [this.Lastlist.monday, this.Lastlist.tuesday, this.Lastlist.wensday, this.Lastlist.thursday,
								this.Lastlist.friday, this.Lastlist.saturday, this.Lastlist.sunday
							],
							markPoint: {
								data: [{
										type: 'max',
										name: this.$t('max')
									},
									{
										type: 'min',
										name: this.$t('min')
									}
								]
							},
							itemStyle: {
								normal: {
									color: '#febf00',
									lineStyle: {
										color: '#f35600'
									}
								}
							},
						}
					]
				})
			},
		}
	}
</script>

<style>
</style>
