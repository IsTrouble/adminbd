<template>
	<div class="layout">
		<Layout class="test" :style="{minHeight: '100vh',width: screenwidth+'px'}" style="" id="layout">
			<Sider :style="{minHeight: '100vh',background:bottomcolor,color:fontcolor}" hide-trigger collapsible :collapsed-width="78" v-model="isCollapsed" v-if="!full">
				<Menu ref="side1" :class="menuitemClasses" :theme="bg3" width="auto" @on-select="go" :active-name="active" :style="{background:bottomcolor,color:fontcolor}">
					<div style="width: 100%;height: 64px;" :style="'background:'+bg1" id='123' name="123">
						<img :src="logo" onerror="src='../../static/logo-menu.png';onerror=''" style="padding-left: 30%;cursor: pointer;width: 66%;height:100%">
					</div>
					<template>
						<div class="user-panel">
							<Col span="8" >
								<img :src="portrait" class="img-circle" alt="User Image" onerror="src='../../static/admin.jpg';onerror=''">
							</Col>
							<Col span="16" style="padding-top: 10px;">
								<p>&nbsp;&nbsp;{{info.nickname}}</p>
							</Col>
						</div>
					</template>
					<template v-for="item in menu" v-if="!item.sub" >
						<MenuItem  :key="item.name" :name="item.name" :style="{}" v-if="item.key" style="font-size:13px">
							<i :key="item.name" :class="item.icon" size="16" style="margin-left: -15px;"></i>
							{{$t(isCollapsed?'':item.label.toString())}}
						</MenuItem >
					</template>
					<template v-else>
						<Submenu :name="item.name" v-if="item.key" style="font-size:13px">
							<template slot="title" >
								<i :key="item.name" :class="item.icon" size="16" style="margin-left: -15px;"></i>
								<Badge v-if="item.count" :count="item.count" class-name="badge-sub-alone" :dot="true">
									{{$t(isCollapsed?'':item.label.toString())}}
								</Badge>
								<i v-else>{{$t(isCollapsed?'':item.label.toString())}}</i>
							</template>
							<Menu-item class="submenu" v-for="sub in item.sub" :key="sub.name" :style="{color:fontcolor}" :name="sub.name" v-if="sub.key" style="margin-left:-7px;font-size:13px">
								<Badge class-name="badge-alone" overflow-count="99" :count="sub.count?sub.count:0">{{$t(sub.label.toString())}}</Badge>
							</Menu-item>
						</Submenu>
					</template>
				</Menu>
			</Sider>
			<Layout>
				<Header  class="m-header" v-if="!full" :style="'background:'+bg2">
					<Row>
						<Col span="20">
							&nbsp;
						</Col>
						<!-- <Col span="3">
							<Dropdown class="layout-header-user fr" @on-click="changelang" trigger="click" >
								<Button type="primary" long class="w-button" :style="'background:'+bg2">
									<Col span="5">
										Language:
									</Col>
									<Col span="19">
										<p style="color: white;width: 100%;">{{$t("lang")}}</p>
									</Col>
								</Button>
								<Dropdown-menu slot="list">
									<Dropdown-item :name="1">中文</Dropdown-item>
									<Dropdown-item :name="2">English</Dropdown-item>
								</Dropdown-menu>
							</Dropdown>
						</Col> -->
						<Col span="3">
							<Dropdown class="layout-header-user fr" @on-click="logout" trigger="click" >
								<Button type="primary" long class="w-button" :style="'background:'+bg2">
									<Col span="5">
										<img :src="portrait" class="img-circle2" alt="User Image" onerror="src='../../static/admin.jpg'">
									</Col>
									<Col span="19">
										<p style="color: white;width: 100%;">{{info.nickname}}</p>
									</Col>
								</Button>
								<Dropdown-menu slot="list">
									<Dropdown-item :name="4">{{$t("Personal Information")}}</Dropdown-item>
									<Dropdown-item :name="6">{{$t("Change Password")}}</Dropdown-item>
									<Dropdown-item :name="5">{{$t("WeChat Follow")}}</Dropdown-item>
									<Dropdown-item :name="3">{{$t("Logout")}}</Dropdown-item>
								</Dropdown-menu>
							</Dropdown>
						</Col>
						<Col span="1" style="">
							<Button style="border:0;background:transparent;padding-left: 20px;right: 0px;font-size:12px" size="small" @click="fullscreen()">
								<div style="color:#ffffff"><icon name="full" width="10px" height="10px" slot="prepend"></icon>&nbsp;{{$t("Fullscreen")}}</div>
							</Button>
						</Col>
					</Row>
				</Header>
				<Content :style="{padding: '0 4px 4px',position:'relative',minHeight: '91vh'}" >
					<div class="layout-content-main">
						<transition name="fade">
							<router-view></router-view>
						</transition>
					</div>
				</Content>
				<div @click="quitfull()" v-if='full' style="height:15px;" @mouseover="show(1)">
					<Col span='22'>
						<div @mouseout="show(2)">&nbsp;</div>
					</Col>
					<Col span='2' v-if='quit' @mouseout="show(2)" style="cursor: pointer;">
						<div @mouseout="show(2)">
							<icon name="quit" width="10" height="10" slot="prepend" ></icon> {{$t("Quit")}}
						</div>
					</Col>
				</div>
			</Layout>
		</Layout>
	</div>
</template>

<script>
	import bus from './bus';
	import Vue from "vue";
	export default {
		data() {
			const validateOldPassCheck = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请再次输入密码'));
				} else if (value !== this.password.password) {
					callback(new Error('两次输入密码不一致!'));
				} else {
					callback();
				}
			};
			return {
				bg1:'#367fa9',
				bg2:'#3c8dbc',
				bg3:"dark",
				bottomcolor:"#1e282c",
				fontcolor:"#ffffff",
				full:false,
				quit:false,
				tagsList: [],
				portrait:'../../static/admin.jpg',
				isCollapsed: false,
				modal: false,
				logo:'../../static/logo-menu.png',
				modalType: 0,
				username:this.global.username,
				info: {
					nickname: '',
					phone: ''
				},
				password: {
					oldPassword: '',
					password: ''
				},
				passwordRules: {
					oldPassword: [{
						required: true,
						message: '旧密码不能为空',
						trigger: 'blur'
					},{
						min: 5,
						message: '密码不能少于6个字符',
						trigger: 'blur'
					}],
					password: [{
						required: true,
						message: '新密码不能为空',
						trigger: 'blur'
					},{
						min: 5,
						message: '密码不能少于6个字符',
						trigger: 'blur'
					}],
					confirm: [{
						validator: validateOldPassCheck,
						required: true,
						trigger: 'blur'
					}],
				},
				role: {
					0: '管理员',
				},
				count: null,
				screenheight:(document.documentElement.clientHeight-80).toString()+'px',
				screenwidth:'',
				active: this.$route.path.split('/')[1],
				menu: [{
					name: 'dashboard',
					icon: 'fa fa-dashboard',
					label: 'Dashboard',
					key:false,
				},{
					name: 'menu',
					icon: 'fa fa-map-o',
					label: 'Operation Monitor',
					key:false,
					sub:[{
						name:'map',
						label:'Operation State',
						key:false,
					},{
						name:'laddermap',
						label:'Ladder State',
						key:false,
					},]
				},{
					name: 'maintain',
					icon: 'fa fa-cogs',
					label: 'Work Flow',
					key:false,
					sub: [{
						name: 'auditinglist',
						label: 'Examine List',
						key:false,
					},{
						name: 'maintain',
						label: 'Order List',
						key:false,
					},{
						name: 'maintainList',
						label: 'Dispatch List',
						key:false,
					}],
				},{
					name: 'event',
					icon: 'fa fa-list-alt',
					label: 'Basic Information',
					key:false,
					sub: [{
						name: 'alList',
						label: 'Devices Information',
						key:false,
					},{
						name:'evolution',
						label:'Firmware Update',
						key:false,
					},{
						name:'ladder',
						label:'Elevator Information',
						key:false,
					}],
				},{
					name: 'group',
					icon: 'fa fa-group',
					label: 'Group',
					key:true,
					sub: [{
						name:'elevator',
						label:'Elevator Group',
						key:true,
					},{
						name: 'organize',
						label: 'Member Group',
						key:true,
					}],
				},{
					name: 'system',
					icon: 'fa fa-address-card-o',
					label: 'System',
					key:true,
					sub:[{
						name: 'userManage',
						label: 'User Management',
						key:false,
					},{
						name: 'inform',
						label: 'Notification Record',
						key:false,
					},{
						name: 'instructions',
						label: 'Instructions',
						key:true,
					},{
						name: 'authority',
						label: 'Permission Assignment',
						key:false,
					}]
				},{
					name: 'setting',
					icon: 'fa fa-cog',
					label: 'Default Setting',
					key:false,
					sub:[{
						name:'print',
						key:false,
						label:'Print QR Code'
					}],
				},{
					name: 'dashboard2',
					icon: 'fa fa-dashboard',
					label: 'Dashboard2',
					key:true,
				}],
				menus:{},
				roles:0,
				lang:'',
			}
		},
		computed: {
			menuitemClasses: function() {
				return [
					'menu-item',
					this.isCollapsed ? 'collapsed-menu' : ''
				]
			},
		},
		mounted(){
			window.onresize = () =>{
				document.getElementById('layout').style.width=document.documentElement.clientWidth+'px'
			};
		},
		async beforeCreate(){
			const val = await this.$api.people({
				id:window.localStorage.getItem('id'),
				num:1,
				page:1,
			})
			this.roles = val.data.data.list[0].role
			window.localStorage.setItem("role",this.roles)
		},
		created(){
			console.log(document.getElementById("123"))
			this.getMenu()
			this.screenwidth=document.documentElement.clientWidth*1
			this.getportrait()
			this.getFunction()
		},
		methods: {
			imgerrorfun1(){
				var img=event.srcElement; 
				img.src='../../static/logo-menu.png';
				img.onerror=null;
			},
			imgerrorfun2(){
				var img=event.srcElement; 
				img.src='../../static/admin.jpg';
				img.onerror=null;
			},
			getMenu(){
				this.menus = JSON.parse(window.localStorage.getItem('menu'))
				if(this.menus.dashboard == true){
					this.menu[0].key = true
				}
				if(this.menus.menu == true){
					this.menu[1].key = true
				}
				if(this.menus.map == true){
					this.menu[1].sub[0].key = true
				}
				if(this.menus.laddermap == true){
					this.menu[1].sub[1].key = true
				}
				if(this.menus.auditinglist== true){
					this.menu[2].sub[0].key = true
				}
				if(this.menus.maintain== true){
					this.menu[2].sub[1].key = true
				}
				if(this.menus.maintainlist == true){
					this.menu[2].sub[2].key = true
				}
				if(this.menus.maintainlist == true||this.menus.auditinglist == true||this.menus.maintain == true){
					this.menu[2].key = true
				}
				if(this.menus.allist == true||this.menus.evolution == true||this.menus.ladder == true){
					this.menu[3].key = true
				}
				if(this.menus.allist == true){
					this.menu[3].sub[0].key = true
				}
				if(this.menus.evolution == true){
					this.menu[3].sub[1].key = true
				}
				if(this.menus.ladder == true){
					this.menu[3].sub[2].key = true
				}
				// if(this.menus.user_manage == true){
				// 	this.menu[4].sub[0].key = true
				// }
				// if(this.menus.inform == true){
				// 	this.menu[4].sub[1].key = true
				// }
				// if(this.menus.authority == true){
				// 	this.menu[4].sub[2].key = true
				// }
				if(this.menus.user_manage == true){
					this.menu[5].sub[0].key = true
				}
				if(this.menus.inform == true){
					this.menu[5].sub[1].key = true
				}
				if(this.menus.authority == true){
					this.menu[5].sub[3].key = true
				}
				if(this.menus.print == true){
					this.menu[6].key = true
					this.menu[6].sub[0].key = true
				}
				this.$forceUpdate()
			},
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
			fullscreen(){

				this.full=true
				var el = document.documentElement;
				var rfs = el.requestFullScreen || el.webkitRequestFullScreen || el.mozRequestFullScreen || el.msRequestFullscreen;      
                if(typeof rfs != "undefined" && rfs) {
                    rfs.call(el);
                };
			},
			quitfull(){
				this.full=false
				if (document.exitFullscreen) {  
					document.exitFullscreen();  
				}  
				else if (document.mozCancelFullScreen) {  
					document.mozCancelFullScreen();  
				}  
				else if (document.webkitCancelFullScreen) {  
					document.webkitCancelFullScreen();  
				}  
				else if (document.msExitFullscreen) {  
					document.msExitFullscreen();  
				} 
				if(typeof cfs != "undefined" && cfs) {
					cfs.call(el);
				}
				// document.getElementsByName("123").className.background=this.bg1
				
				console.log(document.getElementsByName("123"))
				
			},
			gethome(){
				this.$router.push({
					name:'index'
				})
			},
			show(val){
				if (val == 1){this.quit=true}
				if (val == 2){this.quit=false}
			},
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
			async logout(index) {
				this.modalType = parseInt(index)
				switch (parseInt(index)) {
					case 1:
						let user = await this.$api.user(this.$cookie.get('id'))
						this.info = user.data.account.data
						this.modal = true
						break;
					case 2:
						this.modal = true
						break;
						break;
					case 3:
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
						break;
					case 4:
						let resp = await this.$api.user({})
						this.$router.push({
							name: 'person'
						})
						break;
					case 6:
						this.$router.push({
							name: 'newpassword'
						})
						break;
					case 5:
						let http = await this.$api.getcode()
						window.location.href= http.data.qrcode.url
				}
			},
			go(name) {
				if (typeof name === 'string')
					this.$router.push({
						name: name
					});
			},
			goHome() {
				this.$router.push({
					name: 'index'
				});
			},	
			isActive(path) {
                return path === this.$route.fullPath;
            },
            handleTags(command){
                command === 'other' ? this.closeOther() : this.closeAll();
            },
			collapsedSider () {
				this.$refs.side1.toggleCollapse();
			},
			emil(){
				this.$router.push({
					name:'inform'
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
	}
</script>

<style lang="scss" scoped>
	.layout {
		border: 1px solid #d7dde4;
		background: #f5f7f9;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.layout-breadcrumb {
		padding: 10px 15px 0;
	}

	.laycontent {
		position: relative;
	}

	.layout-content {
		// position: relative;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 10px;
		margin: 15px;
		overflow: hidden;
		background: #fff;
		border-radius: 4px;
		// min-height: 500px;
		// min-height: 100%;
	}
	.m-header{
		background: #3c8dbc;
		boxShadow: 0 2px 3px 2px rgba(0,0,0,.1);
		min-height:64px;
	}
	.layout-copy {
		position: absolute;
		left: 0;
		right: 0;
		text-align: center;
		padding: 0 0 15px;
		color: #9ea7b4;
		bottom: 0;
	}

	.ivu-row-flex {
		height: 100%;
	}

	.layout-menu-left {
		background: #464c5b;
		// overflow-y: scroll;
	}

	.layout-menu-ul {
		padding-top: 1px;
	}

	.layout-header {
		padding: 0px;
		height: 60px;
		background: #3c8dbc;
	}

	.layout-header-user {
		height: 40px;
		float: left;
	}

	.layout-logo-left {
		width: 90%;
		height: 130px;
		line-height: 30px;
		background: #5b6270;
		border-radius: 3px;
		margin: 15px auto 36px;
		color: #fff;
		font-size: 18px;
		font-weight: bold;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
	}

	.ivu-menu-dark.ivu-menu-vertical .ivu-menu-item {
		color: #FDFEFC;
	}

	.ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-active:not(.ivu-menu-submenu) {
		// color: #39f;
	}
	.submenu {
		padding: 8px 24px;
	}

	.header-titles {
		line-height: 60px;
		padding-left: 20px;
		margin-top: -1%;
	}

	.info {
		display: flex;
		flex-flow: row;
		justify-content: center;
		align-items: center;
	}
	
	.tags {
        position: relative;
        height: 30px;
        overflow: hidden;
        background: #fff;
        padding-right: 120px;
    }

    .tags ul {
        box-sizing: border-box;
        width: 100%;
        height: 100%;
    }

    .tags-li {
        float: left;
        margin: 3px 5px 2px 3px;
        border-radius: 3px;
        font-size: 12px;
        overflow: hidden;
        cursor: pointer;
        height: 23px;
        line-height: 23px;
        border: 1px solid #e9eaec;
        background: #fff;
        padding: 0 5px 0 12px;
        vertical-align: middle;
        color: #666;
        -webkit-transition: all .3s ease-in;
        -moz-transition: all .3s ease-in;
        transition: all .3s ease-in;
    }

    .tags-li:not(.active):hover {
        background: #f8f8f8;
    }

    .tags-li.active {
        color: #fff;
        background: #59f;
    }

    .tags-li-title {
        float: left;
        max-width: 80px;
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
        margin-right: 5px;
        color: #666;
    }

    .tags-li.active .tags-li-title {
        color: #fff;
    }

    .tags-close-box {
 		position: absolute;
        right: 0;
        top: 0;
        box-sizing: border-box;
        padding-top: 1px;
        text-align: center;
        width: 110px;
        height: 30px;
        background: #0ff;
    }
	.w-button{
		min-width: 150px;
		border: #fff;
		height: 64px;
		background-color: #3c8dbc;
	}
	.w-button:hover{
		background-color: #0B93D5;
	}
	.img-circle{
		width: 50px;
		// max-width: 45px;
		height: 50px;
		border-radius: 100%;
	}
	.img-circle2{
		width: 30px;
		// max-width: 45px;
		height: 20px;
		border-radius: 100%;
	}
	.user-panel{
		padding-left: 20%;
	}
	.layout-content-main{
		width:100%;
		background-color: white;
		overflow-y: hidden;
	}
	.test{
		margin-top: -1px;
		margin-left: -1px;
	}
	.box-username{
		display: table-cell;
		vertical-align: middle;
		text-align: center;
		width: 100%;
	}
	Dropdown:hover Button{
		background-color: white;
	}
</style>
