<template>
	<div class="ProgressMain">
		 <select   class="ProgressSelect"  v-model="prostate" placeholder="$t('progress')" @change="getprogress(1)">
			<option key="6" :label="$t('all')" value="6"></option>
			<option key="1" :label="$t('untreated')" value="1"></option>
			<option key="2" :label="$t('examine order')" value="2"></option>
			<option key="3" :label="$t('treating')" value="3"></Option>
			<option key="4" :label="$t('examine finished')" value="4"></option>
			<option key="5" :label="$t('stored')" value="5"></option>
		  </select>
		<Scroll :on-reach-bottom="handleReachBottom2" :height="400" :distance-to-edge="0">
			
			<div class="ProgressCard" v-for="(item, index) in todo" :key="index">
				<Icon type="logo-android" />{{$t('device')}}：{{item.pro}}
				<Icon type="md-time" />{{item.time}}</br>
				<Icon type="md-alert" />{{$t('fault code')}}：E{{item.description}}{{$t('E'+item.description)}}</br>
				<Icon type="md-locate" />{{$t('install address')}}：{{item.addr}}</br>
				{{$t('progress')}}：{{item.state}} 
				<Progress v-if="item.progress=='0%'"  :percent="0" />
				<Progress v-if="item.progress=='10%'" status="active" :percent="10" />
				<Progress v-if="item.progress=='40%'" status="active" :percent="40" />
				<Progress v-if="item.progress=='80%'" status="active" :percent="80" />
				<Progress v-if="item.progress=='100%'" :percent="100" />
			</div>
			
		</Scroll>
		
	</div>
</template>

<script>
	export default{
		data(){
			return{
				progresspage:0,
				prostate:'6',
				todo:[],
			}
		},
		mounted() {
			this.getprogress();
		},
		methods:{
			load () {
				this.getprogress()
				},
			handleReachBottom2(){
				this.getprogress()
			},
			async getprogress(val){
				if (val == 1) {this.todo=[];this.progresspage=0}
				this.progresspage++
				let res = await this.$api.progress({
					page: this.progresspage,
					num: 20,
					state: this.prostate,
					
					// user_id:window.localStorage.getItem('id')
				})
				if (res.data.code === 0) {
					if (res.data.list.length == 0) {
						this.progresspage--
						this.$Notice.warning({
							title: this.$t('tip'),
							desc: this.$t('It is the end！')
						});
					}
					this.data = res.data.list
					for (var i=0;i<res.data.list.length;i++) {
						
						this.getname(i)
					}
				}
				else {
					this.$Notice.error({
						title: this.$t('error'),
						desc: this.$t('fail to get treating progress')
					})
				}
			},
			async getname(val){
				let ech = await this.$api.devices({device_id:this.data[val].device_id,num:10,page:1})
				if (ech.data.data.list.length == 1) {
				if (ech.data.data.list[0].device_name != null){this.data[val].device_name = ech.data.data.list[0].device_name}
				if (ech.data.data.list[0].IMEI != null){this.data[val].IMEI = ech.data.data.list[0].IMEI}
				if (ech.data.data.list[0].install_addr != null){this.data[val].install_addr = ech.data.data.list[0].install_addr}
				if (ech.data.data.list[0].cell_address != null){this.data[val].cell_address = ech.data.data.list[0].cell_address}
				if (ech.data.data.list[0].ip_country != null){this.data[val].ipaddr = ech.data.data.list[0].ip_country+ech.data.data.list[0].ip_region+ech.data.data.list[0].ip_city}
				var t=Date.parse(new Date())-parseInt(this.data[val].create_time)
				var e=''
				if(t>1000){this.data[val].create_time=parseInt(t/1000).toString()+" secs"}
				if(t>60000){this.data[val].create_time=parseInt(t/60000).toString()+" mins"}
				if(t>3600000){this.data[val].create_time=parseInt(t/3600000).toString()+" hours"}
				if(t>86400000){this.data[val].create_time=parseInt(t/86400000).toString()+" days"}
				}
				var pro='0%'
				var code='0'
				var state=this.$t('untreated')
				var e='未填写'
				if (this.data[val].code != null) {
					code=this.data[val].code.toString(16)
					}
				if (this.data[val].state2 == 'examined') {pro='10%';state=this.$t('examine order')}
				if (this.data[val].state2 == 'untreated') {pro='0%';state=this.$t('untreated')}
				if (this.data[val].state == 'examined') {pro='80%';state=this.$t('examine finished')}
				if (this.data[val].state == 'untreated') {pro='40%';state=this.$t('treating')}
				if (this.data[val].state2 == 'treated') {pro='100%';state=this.$t('stored')}
				this.todo.push({pro:this.data[val].device_name,description: code,time:this.data[val].create_time,progress:pro,addr:this.data[val].install_addr,expect:e,type:ech.data.data.list[0].device_type,state:state,num:10*this.progresspage + val})
			},
		}
	}
</script>

<style scoped lang="scss">
	@import "../scss/minxin";
	.ProgressMain{
		height: 100px;
		width: 100%;
	}
	.ProgressSelect{
		width: 100%;
		height: 30px;
		@include bg_color($background-color-theme-default);
		border: none;
		border-radius: 20px;
		font-size: 15px;
		color: #ffffff;
		font-weight: 900;
		text-align: center;
		text-align-last: center;
		outline: none;
	}
	.ProgressSelect:hover{
		background: #007ee4;
		border-radius: 20px;
		outline: none;
	}
	.ProgressSelect:focus{
		@include bg_color($background-color-theme-default);
		border-radius: 20px;
		outline: none;
	}
	.ProgressCard{
		color:#ffffff;
	}
</style>
