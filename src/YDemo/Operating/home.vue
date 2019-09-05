<template>
	<div class="OHomeMain">
		<div class="HomeUserBox">
			<img :src="portrait" alt="User Image" onerror="src='../../static/admin.jpg';onerror=''">
			<button class="LeftButton" @click="HomeShow('person')">
				个人
			</button>
			<button class="RightButton" @click="logout">
				退出
			</button>
		</div>
		<div class="OHomeSlider">
			<div class="SliderBtnList">
				<button @click="HomeShow('map')" class="SliderBtn">运行状态</button>
				<button @click="HomeShow('laddermap')" class="SliderBtn">电梯状态</button>
				<button @click="HomeShow('auditinglist')" class="SliderBtn">审核列表</button>
				<button @click="HomeShow('maintain')" class="SliderBtn">工单列表</button>
				<button @click="HomeShow('maintainList')" class="SliderBtn">维保列表</button>
				<button @click="HomeShow('alList')" class="SliderBtn">设备信息</button>
				<button @click="HomeShow('evolution')" class="SliderBtn">固件更新</button>
				<button @click="HomeShow('ladder')" class="SliderBtn">电梯信息</button>
				<button @click="HomeShow('elevator')" class="SliderBtn">电梯组</button>
				<button @click="HomeShow('organize')" class="SliderBtn">人员组</button>
				<button @click="HomeShow('userManage')" class="SliderBtn">用户管理</button>
				<button @click="HomeShow('inform')" class="SliderBtn">通知记录</button>
				<button @click="HomeShow('instructions')" class="SliderBtn">说明文档</button>
				<button @click="HomeShow('authority')" class="SliderBtn">权限管理</button>
				<button @click="HomeShow('print')" class="SliderBtn">二维码</button>
				<button @click="HomeShow('dashboard2')" class="SliderBtn">掉线监控</button>
				<button @click="HomeShow('dashboard')" class="SliderBtn">返回展示页</button>
			</div>
		</div>
		<div class="OHContent">
			<transition name="move" mode="out-in">
			    <keep-alive :include="tagsList">
			        <router-view></router-view>
			    </keep-alive>
			</transition>
		</div>
	</div>
</template>

<script>
	import ThemeSelector from '@/YDemo/Component/ThemeSelector'
	export default{
		data(){
			return{
				isshow:false,
				showdata:'1',
				portrait:'../../static/admin.jpg',
			}
		},
		created() {
			var flag=window.localStorage.getItem('id');
			if(flag==null)
			{
				this.$router.push({
					name:"login"
				})
				this.$Message.error("请先登录！！！");
			}
			this.getportrait();
			this.getFunction()
		},
		mounted() {
			
		},
		methods:{
			async getFunction(){
				const res = await this.$api.getFunction({
					page:1,
					num:1,
					id:this.global.roles,
				})
				if(res.data.code == 0){
					this.global.functions = res.data.data.list[0]
				}
			},
			async logout() {
				let res = await this.$api.logout({})
				window.$cookie.delete('id')
				window.$cookie.delete('role')
				window.localStorage.removeItem('id');
				window.localStorage.removeItem('menu');
				// window.localStorage.removeItem('username');
				this.global.username = ''
				this.$router.replace({
					name: 'login'
				})
			},
			MenuShow(){
				this.isshow=!this.isshow;
			},
			HomeShow(index){
				this.showdata=index;
				this.$router.push({
					name:index
				})
			},
			async getportrait(){
				let res = await this.$api.people({id:window.localStorage.getItem('id'),num:1,page:1})
				if (0 === res.data.code) {
					if (res.data.data.list[0].portrait != null) {
						this.portrait='http://server.asynciot.com/getfile?filePath='+res.data.data.list[0].portrait
						}
			
					if (res.data.data.list[0].nickname != null) {
						this.info.nickname=res.data.data.list[0].nickname
					}else {
						this.info.nickname=res.data.data.list[0].username
					}
					
					res = await this.$api.readOrganize({
						id:res.data.data.list[0].organization_id.split(';')[0],
						nums:1,
						page:1,
					})
					if (res.data.data.list.length>0){
						if (res.data.data.list[0].bg1 !=null) {
							this.bg1=res.data.data.list[0].bg1
							this.bg2=res.data.data.list[0].bg2
						}
					// console.log(res.data.data.list[0].bg3)
					if (res.data.data.list[0].bg3!=null){
						this.bg3=res.data.data.list[0].bg3
						if (this.bg3=="dark"){this.bottomcolor="#1e282c";this.fontcolor="#ffffff"}
						if (this.bg3=="light"){this.bottomcolor="#ffffff";this.fontcolor="#1e282c"}
						if (this.bg3=="primary"){this.bottomcolor="#2d8cf0";this.fontcolor="#ffff22"}
						}
					this.logo='http://server.asynciot.com/getfile?filePath='+res.data.data.list[0].logo
					}
				}
			},
		},
		components:{
			'ThemeSelector':ThemeSelector,
		}
	}
</script>

<style lang="scss" scope>
 @import "../scss/minxin";
.OHomeMain{
	min-height: 900px;
	height:100%;
	width:100%;
	@include bg_color($background-color-theme-default);
	padding: 16px;
	position: relative;
}
.HomeUserBox{
	width: 125px;
	height: 125px;
	@include fg_color($foreground-color-theme-default);
	border-radius: 125px;
	position: relative;
	left: 16px;
	-webkit-box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
	box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
	padding: 10px;
}
.OHomeSlider{
	height: 84%;
	width: 157px;
	@include fg_color($foreground-color-theme-default);
	padding: 16px;
	-webkit-box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
	box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
	position: relative;
}
.OHContent{
	width: 90%;
	height: 94%;
	@include fg_color($foreground-color-theme-default);
	position: absolute;
	left: 190px;
	top: 16px;
	padding: 15px;
}
.SliderBtnList{
	width: 125px;
	height: 480px;
	//background: blue;
	text-align: center;
}
.SliderBtn{
	margin-top: 10px;
	width: 60px;
	height: 60px;
	border-radius: 30px;
	outline: none;
	@include bg_color($background-color-theme-default);
	color: #ffffff;
}
.SliderBtn:focus{
	outline: none;
}
.HomeUserBox img{
	width: 105px;
	height: 50px;
	border-radius:60px 60px 0 0; 
}
.LeftButton{
	width: 50px;
	height: 50px;
	border-radius: 0 0 0 60px;
	margin-bottom: 5px;
	padding: 5px;
	@include bg_color($background-color-theme-default);
	color: #ffffff;
	border: none;
}
.RightButton{
	width: 50px;
	height: 50px;
	border-radius: 0 0 60px 0;
	margin-top: 5px;
	padding: 5px;
	@include bg_color($background-color-theme-default);
	color: #ffffff;
	border: none;
}
.LeftButton:focus{
	background: #0084ff;
	outline: none;
}
.RightButton:focus{
	background: #0084ff;
	outline: none;
}
</style>
