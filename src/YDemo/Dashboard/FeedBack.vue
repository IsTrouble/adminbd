<template>
	<div class="feedbackmain">
		<Scroll :on-reach-bottom="handleReachBottom" :height="250" :distance-to-edge="0">
			<div  v-for="(item,index) in chatlist" :key="index">
				<div class="feedbackchatbox">
					<div class="userinfobox">
						<img :src="item.portrait" alt="user image" 
						style="width: 40px;height: 40px;border-radius: 20px;" 
						onerror="src='../../../static/admin.jpg'">
						<p style="color: #ffffff;font-size: 12px;">{{item.username}}</p>
						<p style="color: #ffffff;font-size: 12px;">{{item.create_time}}</p>
					</div>
					<div class="chatcontentbox">
						{{item.content}}
					</div>
					<div class="chatbuttonbox">
						<button  @click="recall(item.id)" v-if="item.from_id">{{$t("withdraw")}}</button>
						<button  @click="item.showlist=true" v-if="!item.showlist" style="outline: none;"><span style="color: red;">{{item.followlist.length}}</span>{{$t("replies")}}</button>
						<button  @click="item.showlist=false" v-if="item.showlist" style="outline: none;color: #ffffff;">{{$t("hidden reply")}}</button>
						<button  @click="$t('respond ')+item.username+':';chatoptions.follow=item.id;chatoptions.content=''" v-if="chatoptions.follow!=item.id" style="outline: none;">{{$t("respond here")}}</button>
						<button  @click="$t('Please leave your questions and suggestions');chatoptions.follow=-1;chatoptions.content=''" v-if="chatoptions.follow==item.id" style="color:#ffffff">{{$t("new message")}}</button>
					</div>
					<div class="backchatbox" v-for="(follow,index) in item.followlist" :key="index" v-if="item.showlist">
						<div class="chatbox2" style="background: #0359a2;margin: 10px;min-height: 50px;color: #50d2ae;padding: 10px;">
							{{follow.username}}(<i class="fa fa-clock-o"></i>{{follow.create_time}}):</br>
							{{follow.content}}</br>
							<button @click="recall(follow.id)" v-if="follow.from_id">{{$t("withdraw")}}</button>
						</div>
					</div>	
					<div class="chatboxline"></div>
				</div>
			</div>
		</Scroll>
		<form action="#" method="post" style="margin-top:5px">
			<div class="input-group" style="width: 100%;height: 50px;margin-top: 10px;">
				<Col span='18'><input type="text"  name="message" class="FBTT" style="height:40px;border: none;width: 98%;" v-model="chatoptions.content"/></Col>
				<Col span='6'>
				<button type="button" class="FBCallButton" @click="sentchat()" v-if="chatoptions.follow!=-1" :disabled="btn2">{{$t("respond")}}</button>
				<button type="button" class="FBCallButton" @click="sentchat()" v-if="chatoptions.follow==-1" :disabled="btn2">{{$t("message")}}</button>
				</Col>
			</div>
		</form>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				chatoptions: {
					fromId:window.localStorage.getItem('id'),
					title:'1',
					content:'',
					info:'1',
					type:0,
					follow: -1,
				},
				chatlist:[],
				chatpage:0,
				btn2:false,
			}
		},
		mounted(){
			this.getchat();
		},
		methods:{
			handleReachBottom(){
				this.getchat();
			},
			async recall(val){
				let res = await this.$api.delchat({id:val})
				if (res.data.code == 0){
					this.chatpage=0
					this.chatlist=[]
					this.getchat()
					this.$Notice.success({
						title: this.$t('success'),
						desc: this.$t('You has callback the message')
					});
				}
			},
			async sentchat(){
				var res
				if (!/\S/.test(this.chatoptions.content)) {
					this.$Notice.warning({
						title: this.$t('tip'),
						desc: this.$t('the content can not be empty')
					});
				}
				else {
					this.btn2= true
					res= await this.$api.sentchat(this.chatoptions)
					this.btn2= false
				}
				if (res.data.code == 0){
					this.chatpage=0
					this.chatlist=[]
					this.getchat()
					this.$Notice.success({
						title: this.$t('success'),
						desc: this.$t('send message successfully')
					});
					this.chatoptions.content=''
				}
				else{
					this.$Notice.error({
						title: this.$t('error'),
						desc: this.$t('fail to send message')
					});
				}
			},
			async getchat(){
				this.chatpage++
				let cht = await this.$api.chat({num:10,page:this.chatpage,follow:-1})
				if (cht.body.code == 0){
					if (cht.body.list.length == 0) {
						this.chatpage--
						this.$Notice.warning({
							title: this.$t('tip'),
							desc: this.$t('It is the end！'),
						});
					}
					for(var i=0;i<cht.body.list.length;i++){
						var followlist=[]
						let ech = await this.$api.chat({num:100,page:1,follow:cht.body.list[i].id})
						if (ech.body.code == 0){
							followlist=ech.body.list
							for(var j=0;j<followlist.length;j++){
								var t=Date.parse(new Date())-followlist[j].create_time
								if(t<86400000){followlist[j].create_time=this.$format(followlist[j].create_time,'HH:mm')}
								else if(t<31536000000){followlist[j].create_time=this.$format(followlist[j].create_time,'MM-DD')}
								else {followlist[j].create_time=this.$format(followlist[j].create_time,'YYYY-MM-DD')}
								if (followlist[j].nickname != null) {followlist[j].username = followlist[j].nickname}
								if (followlist[j].from_id == window.localStorage.getItem('id')) {followlist[j].from_id=true}
								else {followlist[j].from_id=false}
							}
						}
						if (cht.body.list[i].nickname != null) {
							cht.body.list[i].username = cht.body.list[i].nickname
							}
						var portrait="src='../../../static/admin.jpg'"
						if (cht.body.list[i].portrait != null) {portrait='http://server.asynciot.com/getfile?filePath='+cht.body.list[i].portrait}
						var t=Date.parse(new Date())-cht.body.list[i].create_time
						if(t<86400000){cht.body.list[i].create_time=this.$format(cht.body.list[i].create_time,'HH:mm')}
						else if(t<31536000000){cht.body.list[i].create_time=this.$format(cht.body.list[i].create_time,'MM-DD')}
						else {cht.body.list[i].create_time=this.$format(cht.body.list[i].create_time,'YYYY-MM-DD')}
						if (cht.body.list[i].from_id == window.localStorage.getItem('id')) {cht.body.list[i].from_id=true}
						else {cht.body.list[i].from_id=false}
						this.chatlist.push({id:cht.body.list[i].id,username:cht.body.list[i].username,content:cht.body.list[i].content,create_time:cht.body.list[i].create_time,followlist:followlist,showlist:false,portrait:portrait,from_id:cht.body.list[i].from_id})
					
					}
				}
				else {
					this.$Notice.error({
						title: this.$t('error'),
						desc: this.$t('fail to get message')
					})
				}
			},
			
		}
	}
</script>

<style scope lang="scss">
	@import "../scss/minxin";
.feedbackmain{
	width: 100%;
	height: 100%;
}
.feedbackchatbox{
	width: 100%;
	min-height: 80px;
	padding: 5px;
	margin-top: 5px;
}
.userinfobox{
	height: 20%;
	width: 20%;
	text-align: center;
	float: left;
}
.chatcontentbox{
	min-height: 50px;
	width: 70%;
	color: #ffffff;
	font-size: 12px;
	float: left;
	text-align: center;
	margin-left: 15px;
	@include bg_color($background-color-theme-default);
	padding: 8px;
}
.chatbuttonbox{
	text-align: center;
	position: relative;
	top:10px;
}
.backchatbox{
	margin-top:20px; 
}
.chatbuttonbox button{
	margin-left: 10px;
	border-radius:20px; 
	width: auto;
	height: auto;
	border: none;
	@include bg_color($background-color-theme-default);
	color: #ffffff;
	font-size: 12px;
	padding: 5px;
}
.chatboxline{
	width: 100%;
	height: 3px;
	@include bg_color($background-color-theme-default);
	margin-top:20px; 
}
.FBCallButton{
	height:40px;
	width:80px;
	border: none;
	@include bg_color($background-color-theme-default);
	color:#ffffff;
	border-radius: 0 20px 20px 0;
	outline: none;
}
.FBCallButton:focus{
	outline: none;
}
.FBTT{
	border-radius:20px 0 0 20px; 
	@include bg_color($background-color-theme-default);
	padding-left: 18px;
	color:white;
	 font-size:15px; 
}
.FBTT:focus{
	outline: none;
}
</style>
