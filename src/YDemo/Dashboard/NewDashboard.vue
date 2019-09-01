<template>
	<div class="main">
		<div class="DemoHeader">
			 <div class="DemoLogo">
				  <router-link to="/information/ladder" tag="h2" class="pointer">
				    <Icon style="color: white;" type="md-desktop" />
					 <span style="font-size: 25px;color:#ffffff;">{{$t("Ningbo Shenling Elevator Co., Ltd.")}}</span>
				  </router-link>
			</div>
			
			  <div class="dashboard-header-pic" style="margin-right: -10px;">
			   
			    <span class="center">
			      <img src="../../assets/images/logo.png" alt="logo">
			    </span>
			   
			  </div>
			  <div class="DemoThemeSelector">
				  <ThemeSelector></ThemeSelector>
			  </div>
			  
			  <!-- <div class="LanguageChange">
			  	<Dropdown class="layout-header-user fr" @on-click="changelang" trigger="click" >
			  	
			  	<div>
			  		<img style="height: 20px;width: 20px;" src="../../assets/demo/img/language.png" alt="">
			  	</div>
			  	
			  	<Dropdown-menu slot="list">
			  		<Dropdown-item :name="1">中文</Dropdown-item>
			  		<Dropdown-item :name="2">English</Dropdown-item>
			  	</Dropdown-menu>
			  </Dropdown>	
			  </div> -->
			  <div class="DemoClock">
				  <p class="dashboard-header-clock"><img src="../../assets/images/time.png" style="height: 20px;width: 20px;margin-bottom: 5px;" alt=""> <b v-text="clock"></b></p>
				</div>
			  
		</div>
		<div class="mycontent" >
			<Row :gutter="16">
				<Col  span="6">
					<div class="mycard" style="height: 350px;">
						<div class="card_title">
							<img style="height: 20px;width: 20px;float: left;" src="../../assets/images/talk.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Feedback")}}</p>
						</div>
						<div class="card_content" style="height: 200px;">
							<FeedBack></FeedBack>
						</div>
					</div>
					<div class="mycard" >
						<div class="card_title">
							<img style="height: 20px;width: 20px;float: left;" src="../../assets/images/warning.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Common Faults")}}</p>
						</div>
						<div class="card_content" style="height: 420px;">
							<PieChart></PieChart>
						</div>
					</div>
				</Col>
				<Col span="12">
					<div class="mycard">
						<div class="card_title">
							<img style="width: 18px;float: left;" src="../../assets/images/postiton.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Devices Location")}}</p>
						</div>
						<div class="card_content">
							<mydemomap></mydemomap>
						</div>
					</div>
					
					<div class="mycard" style="height: 370px;">
						<div class="card_title">
							<img style="height: 20px;width: 20px;float: left;margin-top: 1px;" src="../../assets/images/flow.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Equipment Use && Equipment failure comparison")}}</p>
						</div>
						<div class="card_content" style="float: left;width: 100%;overflow:hidden;">
							<Row :gutter="16">
								<Col span="12">
									<BarChart></BarChart>
								</Col>
								<Col span="12">
									<LineChart></LineChart>
								</Col>
							</Row>
						</div>
					</div>
				</Col>
				<Col span="6">
					<div class="mycard" style="height: auto;">
						<div class="card_title">
							<img style="height: 15px;width: 20px;float: left;margin-top: 3px;" src="../../assets/images/device.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Equipment Operation Status")}}</p>
						</div>
						<div class="card_content">
							<InfoCard></InfoCard>
						</div>
					</div>
					<div class="mycard" style="height: 490px;">
						<div class="card_title">
							<img style="height: 20px;width: 20px;float: left;margin-top: 1px;" src="../../assets/images/progress.png" alt="">
							<p style="font-size:15px;color: #ffffff;float: left;font-weight: 600; margin-left: 10px;">{{$t("Treating Progress")}}</p>
						</div>
						<div class="card_content">
							<Progress></Progress>
						</div>
					</div>
				</Col>
			</Row>
		
		</div>
	</div>
</template>

<script>
	import mydemomap from '@/YDemo/Dashboard/mydemomap'
	import Map from '@/YDemo/Dashboard/Map'
	import amap from '@/YDemo/Dashboard/amap'
	import PieChart from '@/YDemo/Dashboard/PieChart'
	import InfoCard from '@/YDemo/Dashboard/InfoCard'
	import Progress from '@/YDemo/Dashboard/Progress'
	import BarChart from '@/YDemo/Dashboard/BarChart'
	import FeedBack from '@/YDemo/Dashboard/FeedBack'
	import LineChart from '@/YDemo/Dashboard/LineChart'
	import ThemeSelector from '@/YDemo/Component/ThemeSelector'
	
	export default{
		
		components: {
			// mydemomap:resolve => {
			// require(['@/YDemo/Dashboard/mydemomap'],resolve)
			// }, 
			'mydemomap':mydemomap,
			//'amap':amap,
			//'Map': Map,
			'PieChart':PieChart,
			'InfoCard':InfoCard,
			'Progress':Progress,
			'BarChart':BarChart,
			'FeedBack':FeedBack,
			'LineChart':LineChart,
			'ThemeSelector':ThemeSelector,
		},
		data(){
			return{
				clock: new Date().toLocaleString(),
			}
		},
		methods:{
			async changelang(index) {
				this.modalType = parseInt(index)
				switch (parseInt(index)) {
					case 1:
						this.$i18n.locale = 'zh-CN';
						localStorage.setItem('language',this.$i18n.locale)
						this.$router.push(0)
						// Vue.config.lang = 'zh-CN'
						break;
					case 2:
						this.$i18n.locale = 'en-US';
						localStorage.setItem('language',this.$i18n.locale)
						this.$router.push(0)
						// Vue.config.lang = 'en-US'
				}
			},
		},
		created() {
// 		alert("111");
// 		var flag=window.localStorage.getItem('username');
// 		alert(flag);
// 		if(flag==null)
// 		{
// 			this.$router.push({
// 				name: '/',
// 			})
// 		}
		setInterval(()=> {
		  this.clock = new Date().toLocaleString()
		}, 1000)
		},
	}
</script>

<style lang="scss" scope>
	@import "../scss/minxin";
	.main{
		width: auto;
		min-width: 1024px;
		overflow-x:auto; 
		@include bg_color($background-color-theme-default);
		height: 100%;
		min-height:768px;
	}
	.DemoHeader{
		width: 100%;
		height: 60px;
		 @include fg_color($foreground-color-theme-default);
		position: relative;
	}
	.DemoClock{
		height: 20px;
		width: auto;;
		float: left;
		position: absolute;
		top: 20px;
		right: 10px;
		z-index: 10;
		color:#ffffff;
		font-size: 15px;
	}
	.center {
		  display: inline-block;
		  border-radius: 50%;
		  top: 6px;
		  border: none;
		  height: 70px;
		  width: 70px;
		  text-align: center;
		  padding-top: 12px; 
		  @include fg_color($foreground-color-theme-default);
		  box-shadow: 0 1px 0 rgba(29, 44, 71, .7), 0 2px 6px rgba(0, 0, 0, .3);
	}
	.mycontent{
		// overflow-y: auto;
		padding: 30px 15px 10px 15px;
		width: 100%;
		height: 90%;
		position: relative;
		top: 5px;
		//overflow: auto;
	}
	.mycard{
		min-height: 200px;
		border-radius: 2px;
		@include fg_color($foreground-color-theme-default);
		padding: 10px;
		margin-bottom: 12px;
	}
	.card_title{
		width: 100%;
		height: 31px;
	}
	.card_content{
		width: 100%;
		height: 100%;
	}
	::-webkit-scrollbar {
	  width: 0;
	  height: 0;
	  color: transparent;
	}
	.DemoThemeSelector{
		z-index: 2;
		position: absolute;
		top: 20px;
		right: 150px;
	}
	.LanguageChange{
		position: absolute;
		top: 20px;
		right: 210px;
		z-index: 2;
	}
	.dashboard-header-pic{
		height: 80px;
		width: 70px;
		text-align: center;
		img {
		  width: auto;
		  height: 40px;
		  z-index: 1;
		}
		position: absolute;
		top: 20px;
		left: 48%;
		right: 47%;
	}
	.DemoLogo{
		width: 300px;
		height: auto;
		position: absolute;
		top: 10px;
		left: 10px;
	}
</style>
