<template>
	<div class="ThemeSelector">
		<img src="../../assets/images/color.png" class="OTHSImg" alt="" @click="showColor()">
		<div class="ColorList" id="ColorList">
			<ul>
				<li><button class="CLBtn" @click="changeTheme('theme')">默认黑</button></li>	
				<li><button class="CLBtn" @click="changeTheme('theme1')">活力橙</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme2')">草木绿</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme3')">心雨紫</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme4')">星空蓝</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme5')">佳人粉</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme6')">彩虹青</button></li>
				<li><button class="CLBtn" @click="changeTheme('theme7')">阳光金</button></li>
			</ul>
		</div>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		methods:{
			changeTheme(theme){
				window.document.documentElement.setAttribute('data-theme',theme);
				localStorage.setItem('theme', theme);
			},
			showColor(){
				if(window.document.getElementById("ColorList").style.height!="160px")
				{
					window.document.getElementById("ColorList").style.height = "160px";
				}
				else
				{
					window.document.getElementById("ColorList").style.height = "0px";
				}
			}
		},
		mounted() {
			
		}
	}
</script>

<style scope lang="scss">
	@import "../scss/minxin";
	.ThemeSelector{
		width: 100%;
		height: 100%;
	}
	.OTHSImg{
		width: 25px;
		height: 25px; 
	}
	.ColorList{
		border-radius: 10px;
		-moz-border-radius: 10px;
		-webkit-border-radius: 10px;
		-o-border-radius: 10px;
		width: 80px;
		height: 0;
		background: #ffffff;
		transition: all 0.5s;
		-moz-transition: all 0.5s; /* Firefox 4 */
		-webkit-transition: all 0.5s; /* Safari and Chrome */
		-o-transition: all 0.5s; /* Opera */
		overflow: hidden;
		position: relative;
		right: 30px;
	}
	.ColorList ul li{
		width: 100%;
		height: 20px;
	}
	.CLBtn{
		width: 100%;
		height: 20px;
		background: #ffffff;
		border: none ;
		outline: none;
	}
	.CLBtn:hover{
		width: 100%;
		height: 20px;
		background: #ededed;
		border: none ;
		outline: none;
	}
</style>
