<template>
	<div class="YLoginBody">
		<div class="YLoginHeader">
			<img class="YLoginHeaderImg" src="../assets/images/nbsllogo2.png" alt="">

			<button @click="languageChange" style="height: 70px;width: 100px;background: #ec1a23;float: right;
			border: none;font-size: 20px;color:#ffffff;font-weight:400;">
				{{$t('lang')}}
			</button>
			
			<button style="height: 70px;width: 100px;float: right;font-size: 20px;color:#ec1a23;
			font-weight:400;border: none;background: #ffffff;outline: none;" @click="ShowLoginBox">
				{{$t("login")}}
			</button>
		</div>
		
		<Carousel class="LoginShowBox" v-model="value1" loop autoplay dots="none" :autoplay-speed="5000">
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl7.png" alt="">
				</CarouselItem>
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl8.png" alt="">
				</CarouselItem>
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl9.png" alt="">
				</CarouselItem>
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl1.png" alt="">
				</CarouselItem>
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl2.png" alt="">
				</CarouselItem>
				<CarouselItem>
					<img class="LoginShowImg" src="../assets/images/nbsl3.png" alt="">
				</CarouselItem>
		</Carousel>
		<div class="lowin lowin-red">
				<div class="lowin-brand">
					<img src="../assets/images/logo-menu.png" alt="logo">
				</div>
				<div class="lowin-wrapper">
					<div class="lowin-box lowin-login">
						<div class="lowin-box-inner">
							<Form :rules="rules" ref='form' :model="form">
								<p>{{$t("LoginWelcome")}}</p>
								<div class="lowin-group">
								<Form-item prop="username">
									<label>{{$t("userinfo")}}</label>
		<!-- 							<input type="email" autocomplete="email" name="email" class="lowin-input"> -->
									<Input type="text" v-model="form.username"></Input>
								</Form-item>
								</div>
								<div class="lowin-group password-group">
								<Form-item prop="password">
									<label>{{$t('Password')}} <a class="forgot-link">{{$t("forget_password?")}}</a></label>
		<!-- 							<input type="password" name="password" autocomplete="current-password" class="lowin-input"> -->
									<Input type="password" v-model="form.password" @on-keyup.enter="login('form')"></Input>
								</Form-item>	
								<checkbox v-model="rem" @click="rem=!rem" style="color:#ff6464">{{$t("remember")}}</checkbox>
								</div>
								<div class="lowin-group forgot-group">
								</div>
								<Button class="lowin-btn login-btn" @click="login('form')" :loading="loading">
									{{$t("login")}}
								</Button>
								<div class="text-foot">
									{{$t('No account?')}} <a href="" class="register-link">{{$t("register")}}</a>
								</div>
							</Form>
						</div>
					</div>
			
					<div class="lowin-box lowin-register">
						<div class="lowin-box-inner">
							<Form ref='form1' :model="form1" :rules="rules1">
								<p>{{$t("Let's create your account")}}</p>
								<div class="lowin-group">
								<Form-item prop="username">	
									<label>{{$t('Username')}}</label>
									<Input type="text" v-model="form1.username"></Input>
								</Form-item>
								</div>
								<div class="lowin-group">
								<Form-item prop="password">
									<label>{{$t('Password')}}</label>
									<Input type="password" v-model="form1.password"></Input>
								</Form-item>
								</div>
								<div class="lowin-group">
								<Form-item prop="confirm">
									<label>{{$t('Confirm Password')}}</label>
									<Input type="password" v-model="query.confirm"></Input>
								</Form-item>
								</div>
								<div class="lowin-group">
								<Form-item prop="mobile">
									<label>{{$t('phone number')}}</label>
									<Row :gutter="30">
										<Col span=6>
											<Input v-model="add86" disabled></Input>
										</Col>
										<Col span=18>
											<Input type="text" v-model="form1.mobile" :maxlength="11"></Input>
										</Col>
									</Row>
								</Form-item>
								</div>
								<div class="lowin-group">
								<Form-item prop="verifyCode">
									<label>{{$t('Verification Code')}}</label>
									<Row :gutter="30">
										<Col span=13>
											<Input type="text" v-model="form1.verifyCode" :maxlength="4"></Input>
										</Col>
										<Col span=11>
											<Button @click="sentMessage" style="width: 100%;" v-if="count<=0">{{$t('Get Code')}}</Button>
											<Button @click="sentMessage" style="width: 100%;" v-if="count>0" disabled>{{$t('Get Code')}}({{count}})</Button>
										</Col>
									</Row>	
								</Form-item>
								</div>
								<Button class="lowin-btn" @click="create('form1')">
									{{$t("login")}}
								</Button>
			
								<div class="text-foot">
									{{$t('Existing accounts?')}} <a href="" class="login-link">{{$t("login")}}</a>
								</div>
							</Form>
						</div>
					</div>
					
					<div class="lowin-box lowin-forgot">
						<div class="lowin-box-inner">
							<Form ref='form2' :model="form2" :rules="rules2">
							<p>{{$t("Let's get your account back")}}</p>
							<Form-item prop="mobile">
								<label>{{$t('Your Registered Mobile Phone')}}<a class="login-back-link">{{$t("login")}}</a></label>
							<!-- 	<input type="email" autocomplete="email" name="email" class="lowin-input"> -->
								<Input type="text" v-model="form2.mobile"></Input>
							</Form-item>
							<Form-item prop="verifyCode">
								<label>{{$t('Verification Code')}}</label>
								<Row :gutter="30">
									<Col span="13">
										<Input type="text" v-model="form2.verifyCode"></Input>
									</Col>
									<Col span="11">
										<div>
											<Button @click="sentMessage2" v-if="count<=0" style="width: 100%">{{$t('Get Code')}}</Button>
											<Button @click="sentMessage2" v-if="count>0" style="width: 100%" disabled>{{$t('Get Code')}}({{count}})</Button>
										</div>
									</Col>
								</Row>
							</Form-item>
							<Form-item prop="newpassword">
								<label>{{$t('Please enter a new password.')}}</label>
								<Input type="password" v-model="form2.newpassword"></Input>
							</Form-item>
							<Form-item prop="confirm">
								<label>{{$t('Please repeat the new password.')}}</label>
								<Input type="password" v-model="query2.confirm"></Input>
							</Form-item>
							<Button class="lowin-btn" @click="reset('form2')">
								{{$t("reset password")}}
							</Button>
							</Form>
						</div>
					</div>
					
				</div>
			
				<footer class="lowin-footer">
			
				</footer>
		</div>
		<!-- <div class="LoginBox">
			<div class="LoginBoxLogo" @click="ShowLoginBox">
				<img class="LoginBoxImg" src="../assets/images/logo-menu.png" alt="">
				<div id="LoginRipple1" class="ripple p1"></div>
				<div id="LoginRipple2" class="ripple p2"></div>
				<div id="LoginRipple3" class="ripple p3"></div>
			</div>
			
			<div class="LoginBoxForm" id="LoginBoxForm">
					<span style="color: #ffaaaa;font-size:14px;font-weight: 600;position: relative;top: 60px;left: 95px;">{{$t("LoginWelcome")}}</span></br>
					<span style="color: #ffaaaa;font-size:14px;font-weight: 600;position: relative;top: 80px;left: 20px;">{{$t("userinfo")}}</span>
					
					<input style="width: 320px;height: 50px; background:#fff6f6;border: none;
					border-radius: 3px;color: #ffffff;font-weight: 600;
					font-size: 14px;position: relative;top: 90px;left: 20px;color:#ff647d;padding-left:20px;
					font-weight: 600;padding-right: 20px;outline: none;" type="text" v-model="form.username"></br>
					
					<span style="color: #ffaaaa;font-size:14px;font-weight: 600;position: relative;top: 120px;left: 20px;">{{$t("password")}}</span>
					<span style="color: #ff647d;font-size:14px;font-weight: 600;position: relative;top: 120px;right: 20px;float: right;">{{$t("forgetpassword")}}</span>
					
					<input style="width: 320px;height: 50px; background:#fff6f6;border: none;
					border-radius: 3px;color: #ffffff;font-weight: 600;
					font-size: 14px;position: relative;top: 130px;left: 20px;
					color:#ff647d;padding-left:20px;font-weight: 600;
					padding-right: 20px;outline: none;" type="password" v-model="form.password" @on-keyup.enter="show">
					
					<button style="width: 320px;height: 50px; background:#ff6464;border: none;
					border-radius: 3px;color: #ffffff;font-weight: 600;
					font-size: 14px;position: relative;top: 160px;left: 20px;" id="YLoginBtn" @click="show">
						{{$t("login")}}
					</button></br>
					<span id="spanLogin" style="height: 15px;width: auto;color: #ffaaaa;font-size:14px;font-weight: 600;position: relative;top: 180px;left: 120px;">{{$t("noaccount")}}</span>
					<span id="spanLogin2" style="height: 15px;width: auto;color: #ff647d;font-size:14px;font-weight: 600;position: relative;top: 180px;left: 120px;">{{$t("register")}}</span>
			</div>
			<div class="LoginBoxFormShadow" id="LoginBoxFormShadow"></div>
		</div> -->
		<div class="YLoginFooter">
			<span style="font-size: 14px;font-weight: 600;">© 2003-2018 {{$t("nbsl")}} | {{$t("Telephone consultation")}}：021-37011866</span></br>
			<span style="font-size: 14px;font-weight: 600;">{{$t("site link")}}: <a href="http://www.nbsl.com/">{{$t("nbsl2")}}</a></span>
		</div>
	</div>
</template>

<script>
	
	export default{
		data(){
			const validateOldPassCheck = (rule, value, callback) => {
				if(this.query.confirm == '') {
					callback(new Error(this.$t('Please enter your password again.')));
				} else if(this.query.confirm !== this.form1.password) {
					callback(new Error(this.$t('Inconsistent passwords!')));
				} else {
					callback();
				}
			};
			const validateOldPassCheck2 = (rule, value, callback) => {
				if(this.query2.confirm == '') {
					callback(new Error(this.$t('Please enter your password again.')));
				} else if(this.query2.confirm !== this.form2.newpassword) {
					callback(new Error(this.$t('Inconsistent passwords!')));
				} else {
					callback();
				}
			};
			return{
				rem: false,
				loading: false,
				add86:'+86',
				count: 0,
				count2:0,
				time: '',
				time2:'',
				value1:0,
				form: {
					username: '',
					password: '',
				},
				form1: {
					id:'',
					mobile: '',
					username: '',
					password: '',
					verifyCode:'',
				},
				form2: {
					mobile: '',
					newpassword: '',
					verifyCode:'',
				},
			rules: {
				username: [{
					// required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur',
					pattern:/^[A-Za-z0-9]+$/,
				},{
					required: true,
					message: this.$t("Please fill in the username"),
					trigger: 'blur'
				},
				{
					type: 'string',
					min: 5,
					message: this.$t("Username should not less than 5 letters"),
					trigger: 'blur'
				}],
				password: [{
					required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur',
					pattern:/^[A-Za-z0-9]+$/,
				},
				{
					type: 'string',
					min: 3,
					message: this.$t("Password should not less than 6 letters"),
					trigger: 'blur'
				}],
				veri: [{
					required: true,
					message: this.$t("Please fill in the username"),
					trigger: 'blur'
				},
	// 		  {
	// 		    validator: validateOldPassCheck,
	// 		    required: true,
	// 		    trigger: 'blur'
	// 		  }
				]
			},
			query:{
				confirm:'',
			},
			query2:{
				confirm:'',
			},
			rules1: {
				username: [{
					required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur',
					pattern:/^[\u4e00-\u9fa5_a-zA-Z0-9]+$/,
				},
				{
					type: 'string',
					min: 6,
					message: this.$t('Username should not less than 5 letters'),
					trigger: 'blur'
				}],
				mobile: [{
					required: true,
					message: this.$t('Please fill in the phone number.'),
					trigger: 'blur'
				},{
					message: this.$t('Wrong Phone Number'),
					pattern: /^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/,
					trigger: 'blur'
				}],
				verifyCode: [{
					required: true,
					message: this.$t('Please fill in the true verification code.'),
					trigger: 'blur'
				},
				{
					type: 'string',
					min: 4,
					message: this.$t('Verification code should be 4 letters'),
					trigger: 'blur'
				}],
				password: [{
					required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur'
				},
				{
					type: 'string',
					min: 6,
					message: this.$t('Password should not less than 6 letters'),
					trigger: 'blur'
				}],
				confirm: [{
					validator: validateOldPassCheck,
					required: true,
					trigger: 'blur'
				}],
				
			},		
			rules2: {
				username: [{
					required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur',
					pattern:/^[A-Za-z0-9]+$/,
				},
				{
					type: 'string',
					min: 5,
					message: this.$t('Username should not less than 5 letters'),
					trigger: 'blur'
				}],
				mobile: [{
					required: true,
					message: this.$t('Please fill in the phone number.'),
					trigger: 'blur'
				},{
					message: this.$t('Wrong Phone Number'),
					pattern: /^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/,
					trigger: 'blur'
				}
				],
				verifyCode: [{
					required: true,
					pattern:/^[A-Za-z0-9]+$/,
					message: this.$t('Please fill in the true verification code.'),
					trigger: 'blur'
				},
				{
					type: 'string',
					min: 4,
					message: this.$t('Verification code should be 4 letters'),
					trigger: 'blur'
				}],
				newpassword: [{
					required: true,
					message: this.$t('Illegal characters!'),
					trigger: 'blur',
					pattern:/^[A-Za-z0-9]+$/,
				},
				{
					type: 'string',
					min: 6,
					message: this.$t('Password should not less than 6 letters'),
					trigger: 'blur'
				}],
				confirm: [{
					validator: validateOldPassCheck2,
					required: true,
					trigger: 'blur'
				}],
			},

			}
		},
		methods:{
			ShowLoginBox(){
				if(window.document.getElementById("LoginBoxForm").style.height!="440px")
				{
					window.document.getElementById("LoginBoxForm").style.height = "440px";
					window.document.getElementById("LoginBoxFormShadow").style.height = "100px";
					window.document.getElementById("LoginRipple1").hidden = true;
					window.document.getElementById("LoginRipple2").hidden = true;
					window.document.getElementById("LoginRipple3").hidden = true;
				}
				else
				{
					window.document.getElementById("LoginBoxForm").style.height = "0px";
					window.document.getElementById("LoginBoxFormShadow").style.height = "0px";
					window.document.getElementById("LoginRipple1").hidden = false;
					window.document.getElementById("LoginRipple2").hidden = false;
					window.document.getElementById("LoginRipple3").hidden = false;
				}
			},
			async login(name) {
				window.localStorage.setItem('item','')
				this.loading = true
				this.$refs[name].validate(async (valid) => {
					if (valid) {
						let res = await this.$api.login(this.form)
						if (!res.data.code) {
							this.global.username = res.data.account.username
							window.localStorage.setItem('username',this.global.username)
							window.localStorage.setItem('id',res.data.account.id)
							window.localStorage.setItem('rem',this.rem)
							window.localStorage.setItem('u',this.form.username)
							window.localStorage.setItem('mobile',res.data.account.mobile)
							if (this.rem) {
								window.localStorage.setItem('p',this.form.password)
							}
							const val = await this.$api.people({id:res.data.account.id,num:1,page:1})
							const itm = await this.$api.getMenu({
								page:1,
								num:1,
								id:val.data.data.list[0].role,
							})
							const obj = JSON.stringify(itm.data.data.list[0])
							window.localStorage.setItem('menu',obj)
							this.$Message.success({
								content: this.$t("Login successfully, going the home page!"),
								duration: 0.5,
								onClose: () => {
									this.$router.push({
										name: 'dashboard',
									})
								}
							})
						} else {
							this.loading = false;
							this.$Message.error(this.$t("Login failed!"));
						}
					} else {
						this.loading = false;
						this.$Message.error(this.$t("Please improve login information!"));
					}
				})
			},
			async sentMessage() {
				if (/^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/.test(this.form1.mobile)){
					this.time=Number(Date.parse(new Date()))
					window.localStorage.setItem('munite',this.time)
					this.munite()
					let res = await this.$api.sentMessage(this.form1.mobile)
				}
				else{
					if (this.form1.mobile == null || this.form1.mobile == ''){
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Please fill in the phone number.')
						})
					}
					else {
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Wrong Phone Number')
						})
					}
				}
			},
			async sentMessage2(){
				if (/^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\d{8}$/.test(this.form2.mobile)){
					this.time2=Number(Date.parse(new Date()))
					window.localStorage.setItem('munite',this.time2)
					this.munite()
					let res = await this.$api.sentMessage(this.form2.mobile)
				}
				else{
					if (this.form2.mobile == null || this.form2.mobile == ''){
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Please fill in the phone number.')
						})
					}
					else {
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Wrong Phone Number')
						})
					}
				}
			},
			munite(){
				if (!(/^\d+$/.test(window.localStorage.getItem('munite')))) window.localStorage.setItem('munite',0)
				this.time=Number(Date.parse(new Date()))
				this.count=(Number(window.localStorage.getItem('munite'))+60000-this.time)/1000
				if (this.count>0){		
					setTimeout(()=>{
						this.munite()
					}, 1000)
				}
				else{window.localStorage.setItem('munite',0)}
			},
			create(name) {
				this.loading = true
				this.$refs[name].validate(async(valid) => {
					if(valid) {
						let res = null
						if(!this.$route.params.id) {
							res = await this.$api.register(this.form1)
						}
						console.log(res);
						this.loading = false
						if(res.data.code == 0){
							this.$refs[name].resetFields();
							window.localStorage.setItem('munite',0);
							this.$router.back();
							this.$Notice.success({
								title: this.$t('success'),
								desc: '',
								onClose: () => {
								}
							})
						}else {
							this.loading = false
							this.$Notice.error({
								title: this.$t('error'),
								desc: this.$t('Fail to register')
							})
						}
					}else{
						this.loading = false
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Please check the form!')
						})
					}
				})
			},
			reset(name) {
				this.loading = true
				this.$refs[name].validate(async(valid) => {
					if(valid) {
						let res = null
						if(!this.$route.params.id) {
							res = await this.$api.retrieve(this.form2)
						}
						this.loading = false
						if(res.data.code == 0){
							this.$refs[name].resetFields();
							window.localStorage.setItem('munite',0);
							this.$router.back();
							this.$Notice.success({
								title: this.$t('success'),
								desc: '',
								onClose: () => {
								}
							})
						}else {
							this.loading = false
							this.$Notice.error({
								title: this.$t('error'),
								desc: this.$t('Fail to reset')
							})
						}
					}else{
						this.loading = false
						this.$Notice.error({
							title: this.$t('error'),
							desc: this.$t('Please check the form!')
						})
					}
				})
			},
			async show(){
				document.getElementById("YLoginBtn").disabled=true;
				document.getElementById("YLoginBtn").style.background="#494949";
				let res = await this.$api.login(this.form)
				if (!res.data.code) {
					window.localStorage.setItem('username',res.data.account.username)
					window.localStorage.setItem('id',res.data.account.id)
					window.localStorage.setItem('rem',this.rem)
					window.localStorage.setItem('u',this.form.username)
					window.localStorage.setItem('mobile',res.data.account.mobile)
					if (this.rem) {
						window.localStorage.setItem('p',this.form.password)
					}
					const val = await this.$api.people({id:res.data.account.id,num:1,page:1})
					const itm = await this.$api.getMenu({
						page:1,
						num:1,
						id:val.data.data.list[0].role,
					})
					const obj = JSON.stringify(itm.data.data.list[0])
					window.localStorage.setItem('menu',obj)
					this.$Message.success({
						content: '登录成功，正在跳转!',
						duration: 0.5,
						onClose: () => {
							this.$router.push({
								name: 'demo',
							})
						}
					})
					document.getElementById("YLoginBtn").disabled=false;
					document.getElementById("YLoginBtn").style.background="#ff6464";
				} else {
					this.$Message.error('登录失败!');
					document.getElementById("YLoginBtn").disabled=false;
					document.getElementById("YLoginBtn").style.background="#ff6464";
				}
			},
			async languageChange(){
				if (this.$i18n.locale == 'en-US'){
					this.$i18n.locale = 'zh-CN';
					localStorage.setItem('language',this.$i18n.locale)
					this.$router.go(0)
					// Vue.config.lang = 'zh-CN'
				}
				else{
					this.$i18n.locale = 'en-US';
					localStorage.setItem('language',this.$i18n.locale)
					this.$router.go(0)
					// Vue.config.lang = 'en-US'
				}
			},
			loginandregisterbox(){
				var Auth = {
					vars: {
						lowin: document.querySelector('.lowin'),
						lowin_brand: document.querySelector('.lowin-brand'),
						lowin_wrapper: document.querySelector('.lowin-wrapper'),
						lowin_login: document.querySelector('.lowin-login'),
						lowin_wrapper_height: 0,
						login_back_link: document.querySelector('.login-back-link'),
						forgot_link: document.querySelector('.forgot-link'),
						login_link: document.querySelector('.login-link'),
						login_btn: document.querySelector('.login-btn'),
						register_link: document.querySelector('.register-link'),
						password_group: document.querySelector('.password-group'),
						password_group_height: 0,
						forgot_group: document.querySelector('.forgot-group'),
						forgot_group_height: 0,
						lowin_register: document.querySelector('.lowin-register'),
						lowin_forgot: document.querySelector('.lowin-forgot'),
						lowin_footer: document.querySelector('.lowin-footer'),
						box: document.getElementsByClassName('lowin-box'),
						option: {}
					},
					register(e) {
						Auth.vars.lowin_login.className += ' lowin-animated';
						setTimeout(() => {
							Auth.vars.lowin_login.style.display = 'none';
						}, 500);
						Auth.vars.lowin_register.style.display = 'block';
						Auth.vars.lowin_register.className += ' lowin-animated-flip';

						Auth.setHeight(Auth.vars.lowin_register.offsetHeight + Auth.vars.lowin_footer.offsetHeight);

						e.preventDefault();
					},
					login(e) {
						Auth.vars.lowin_register.classList.remove('lowin-animated-flip');
						Auth.vars.lowin_register.className += ' lowin-animated-flipback';
						Auth.vars.lowin_login.style.display = 'block';
						Auth.vars.lowin_login.classList.remove('lowin-animated');
						Auth.vars.lowin_login.className += ' lowin-animatedback';
						setTimeout(() => {
							Auth.vars.lowin_register.style.display = 'none';
						}, 500);
						
						setTimeout(() => {
							Auth.vars.lowin_register.classList.remove('lowin-animated-flipback');
							Auth.vars.lowin_login.classList.remove('lowin-animatedback');
						},1000);

						Auth.setHeight(Auth.vars.lowin_login.offsetHeight + Auth.vars.lowin_footer.offsetHeight);

						e.preventDefault();
					},
					forgot(e) {
						/* Auth.vars.password_group.classList += ' lowin-animated';
						Auth.vars.login_back_link.style.display = 'block';
						Auth.vars.forgot_group.style.display='block';
						setTimeout(() => {
							Auth.vars.login_back_link.style.opacity = 1;
							Auth.vars.password_group.style.height = 0;
							Auth.vars.password_group.style.margin = 0;
						}, 100);
						
						setTimeout(() => {
							Auth.vars.password_group.style.display='none';
						}, 1000);
						
						Auth.vars.login_btn.innerText = '忘记密码';

						Auth.setHeight(Auth.vars.lowin_wrapper_height - Auth.vars.password_group_height);
						Auth.vars.lowin_login.querySelector('form').setAttribute('action', Auth.vars.option.forgot_url);
						e.preventDefault(); */
						Auth.vars.login_back_link.style.display = 'block';
						setTimeout(() => {
							Auth.vars.login_back_link.style.opacity = 1;
						}, 100);
						Auth.vars.lowin_login.className += ' lowin-animated';
						setTimeout(() => {
							Auth.vars.lowin_login.style.display = 'none';
						}, 500);
						Auth.vars.lowin_forgot.style.display = 'block';
						Auth.vars.lowin_forgot.className += ' lowin-animated-flip';
						
						Auth.setHeight(Auth.vars.lowin_forgot.offsetHeight + Auth.vars.lowin_footer.offsetHeight);
						
						e.preventDefault();
					},
					loginback(e) {
						/* Auth.vars.password_group.classList.remove('lowin-animated');
						Auth.vars.password_group.classList += ' lowin-animated-back';
						Auth.vars.password_group.style.display = 'block';
						Auth.vars.forgot_group.style.display='none';
						setTimeout(() => {
							Auth.vars.login_back_link.style.opacity = 0;
							Auth.vars.password_group.style.height = Auth.vars.password_group_height + 'px';
							Auth.vars.password_group.style.marginBottom = 30 + 'px';
						}, 100);

						setTimeout(() => {
							Auth.vars.login_back_link.style.display = 'none';
							Auth.vars.password_group.classList.remove('lowin-animated-back');
						}, 1000);

						Auth.vars.login_btn.innerText = '登录';
						Auth.vars.lowin_login.querySelector('form').setAttribute('action', Auth.vars.option.login_url);

						Auth.setHeight(Auth.vars.lowin_wrapper_height);
						
						e.preventDefault(); */
						Auth.vars.lowin_forgot.classList.remove('lowin-animated-flip');
						Auth.vars.lowin_forgot.className += ' lowin-animated-flipback';
						Auth.vars.lowin_login.style.display = 'block';
						Auth.vars.lowin_login.classList.remove('lowin-animated');
						Auth.vars.lowin_login.className += ' lowin-animatedback';
						setTimeout(() => {
							Auth.vars.lowin_forgot.style.display = 'none';
						}, 500);
						
						setTimeout(() => {
							Auth.vars.lowin_forgot.classList.remove('lowin-animated-flipback');
							Auth.vars.lowin_login.classList.remove('lowin-animatedback');
						},1000);
						
						Auth.setHeight(Auth.vars.lowin_login.offsetHeight + Auth.vars.lowin_footer.offsetHeight);
						
						e.preventDefault();
					},
					setHeight(height) {
						Auth.vars.lowin_wrapper.style.minHeight = height + 'px';
					},
					brand() {
						Auth.vars.lowin_brand.classList += ' lowin-animated';
						setTimeout(() => {
							Auth.vars.lowin_brand.classList.remove('lowin-animated');
						}, 1000);
					},
					init(option) {
						Auth.setHeight(Auth.vars.box[0].offsetHeight + Auth.vars.lowin_footer.offsetHeight);
						Auth.vars.password_group.style.height = Auth.vars.password_group.offsetHeight + 'px';
						Auth.vars.password_group_height = Auth.vars.password_group.offsetHeight;
						Auth.vars.forgot_group.style.height = Auth.vars.forgot_group.offsetHeight + 'px';
						Auth.vars.forgot_group_height = Auth.vars.forgot_group.offsetHeight;
						Auth.vars.lowin_wrapper_height = Auth.vars.lowin_wrapper.offsetHeight;
						Auth.vars.forgot_group.style.display='none';
						Auth.vars.option = option;
						Auth.vars.lowin_login.querySelector('form').setAttribute('action', option.login_url);

						var len = Auth.vars.box.length - 1;

						for(var i = 0; i <= len; i++) {
							if(i !== 0) {
								Auth.vars.box[i].className += ' lowin-flip';
							}
						}

						Auth.vars.forgot_link.addEventListener("click", (e) => {
							Auth.brand();
							Auth.forgot(e);
						});

						Auth.vars.register_link.addEventListener("click", (e) => {
							Auth.brand();
							Auth.register(e);
						});

						Auth.vars.login_link.addEventListener("click", (e) => {
							Auth.brand();
							Auth.login(e);
						});

						Auth.vars.login_back_link.addEventListener("click", (e) => {
							Auth.brand();
							Auth.loginback(e);
						});
					}
				}
					Auth.init({
					login_url: '#login',
					forgot_url: '#forgot'
				});
			}
		},
		created() {
			this.munite();
			var rem=window.localStorage.getItem('rem')
			var u=window.localStorage.getItem('u')
			if (u != null) {
				this.form.username=u
			}
			if (rem == 'true') {
				this.rem=true
				var u=window.localStorage.getItem('u')
				if (u != null) {
					this.form.username=u
				}
				var p=window.localStorage.getItem('p')
				if (p != null) {
				  this.form.password=p
				}
			}
		},
		mounted(){
			this.loginandregisterbox();
		}
	}
</script>

<style scope lang="scss">
.YLoginBody{
	width: 100%;
	height:100%;
	background-color:#fbfbfb;
	overflow: scroll;
}
.YLoginHeader{
	width: 100%;
	background: #ffffff;
	height: 70px;
}
.YLoginHeaderImg{
	width: auto;
	height: 40px;
	position: relative;
	left: 10px;
	top: 10px;
}
.LoginShowBox{
	height: 850px;
	width: 100%;
	position: relative;
}
.LoginShowImg{
	height: 850px;
	width: 100%;
}
.ripple {
 border-radius: 50%;
 background-clip: padding-box;
 background: transparent;
 width: 280px;
 height: 280px;
 border-radius: 50%;
 position: absolute;
 right: -95px;
 top: -95px;
 z-index: 9;
 margin-bottom: 0px;
}
.p1:after,
.p2:after,
.p3:after {
 content: "";
 display: block;
 position: absolute;
 top: 0;
 right: 0;
 bottom: 0;
 left: 0;
 border-radius: 50%;
 border: 2px solid #ff6464;
 opacity: 0;
 background-color: rgba(255,170, 170, 0.2);
 z-index: -1;
}
.p1:after{
 -webkit-animation: ripple 4.5s ease-out 225ms infinite;
 animation: ripple 4.5s ease-out 225ms infinite;
  z-index: -1;
}
.p2:after{
 -webkit-animation: ripple 4.5s ease-out .9s infinite;
 animation: ripple 4.5s ease-out .9s infinite;
  z-index: -1;
}
.p3:after{
 -webkit-animation: ripple 4.5s ease-out 1.8s infinite;
 animation: ripple 4.5s ease-out 5.8s infinite;
  z-index: -1;
}
@-webkit-keyframes ripple {
 0% {
  opacity: 0;
  -webkit-transform: scale(.1)
 }
 5% {
  opacity: 0;
 }
 35% {
  opacity: 0.5;
 }
 to {
  opacity: 0;
  -webkit-transform: scale(1)
 }
}
@keyframes ripple {
 0% {
  opacity: 0;
  -webkit-transform: scale(.1);
  transform: scale(.1)
 }
 10% {
  opacity: 0;
 }
 35% {
  opacity: 0.5;
 }
 to {
  opacity: 0;
  -webkit-transform: scale(1);
  transform: scale(1)
 }
}

// .LoginBox{
// 	width: 400px;
// 	height: 600px;
// 	background: none;
//  	position: absolute;
// 	right:10%;
// 	top:80px; 
// }
// .LoginBoxLogo{
// 	width: 90px;
// 	height: 90px;
// 	background: #ffffff;
// 	border: nano;
// 	border-radius:50px;
// 	-webkit-box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
// 	box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
// 	z-index: 100;
// 	padding: 10px;
// 	margin: 0 auto;
// 	position: relative;
// 	top:45px;
// 	text-align: center;
// }
// .LoginBoxImg{
// 	width: 60px;
// 	height: 60px;
// 	z-index: 6;
// }
// .LoginBoxForm{
// 	border-radius:3px; 
// 	width: 360px;
// 	height: 0px;
// 	background: #ffffff;
// 	z-index: 2;
// 	position: relative;
// 	margin: 0 auto;
// 	-webkit-box-shadow: 0 7px 25px rgba(0, 0, 0, .08);
// 	box-shadow: 0 7px 25px rgba(0, 0, 0, .08);
// 	overflow: hidden;
// 
// 	transition: all 0.5s ease;
// 	-moz-transition: all 0.5s ease; /* Firefox 4 */
// 	-webkit-transition: all 0.5s ease; /* Safari and Chrome */
// 	-o-transition: all 0.5s ease; /* Opera */
// }
// .LoginBoxFormShadow{
// 	-webkit-box-shadow: 0 0 25px rgba(0, 0, 0, .1);
// 	box-shadow: 0 0 25px rgba(0, 0, 0, .1);
// 	-webkit-transform: translate(0, -92.6%) scale(.88);
// 	-ms-transform: translate(0, -92.6%) scale(.88);
// 	transform: translate(0, -92.6%) scale(.88);
// 	border-radius: 3px;
// 	position:relative;
// 	top: 6px;
// 	margin: 0 auto;
// 	width: 318px;
// 	height: 0px;
// 	background-color: #ffffff;
// 	z-index: 1;
// 	
// 	transition: all 0.5s;
// 	-moz-transition: all 0.5s; /* Firefox 4 */
// 	-webkit-transition: all 0.5s; /* Safari and Chrome */
// 	-o-transition: all 0.5s; /* Opera */
// }
.YLoginFooter{
	width: 100%;
	height: 40px;
	position: relative;
	bottom:20px;
	background: #ffffff;
	text-align: center;
}
	.lowin {
		--color-primary: #44a0b3;
		--color-grey: rgba(68, 160, 179, .06);
		--color-dark: rgba(68, 160, 179, .5);
		--color-semidark: rgba(68, 160, 179, .5);
	
		text-align: center;
		margin: 60px 0 0 0;
		font-family: 'Segoe UI';
		font-size: 14px;
		position: absolute;
		right:10%;
		top:80px; 
	}
	
	.lowin .lowin-wrapper {
		-webkit-transition: all 1s;
		-o-transition: all 1s;
		transition: all 1s;
		-webkit-perspective: 1000px;
		        perspective: 1000px;
		position: relative;
		height: 100%;
		width: 360px;
		margin: 0 auto;
	}
	
	.lowin.lowin-red {
		--color-primary: #ff6464;
		--color-grey: rgba(255, 100, 100, .06);
		--color-dark: rgba(255, 100, 100, .8);
		--color-semidark: rgba(255, 100, 100, .55);
	}
	.lowin a {
		color: var(--color-primary);
		text-decoration: none;
		border-bottom: 1px dashed var(--color-semidark);
		margin-top: -3px;
		padding-bottom: 2px;
	}
	
	.lowin * {
		-webkit-box-sizing: border-box;
		        box-sizing: border-box;
	}
	
	.lowin .lowin-brand {
		overflow: hidden;
		width: 100px;
		height: 100px;
		margin: 0 auto -50px auto;
		border-radius: 50%;
		-webkit-box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
		        box-shadow: 0 4px 40px rgba(0, 0, 0, .07);
		padding: 10px;
		background-color: #fff;
		z-index: 1;
		position: relative;
	}
	
	.lowin .lowin-brand img {
		width: 100%;
	}
	
	.lowin .lowin-box {
		width: 100%;
		position: absolute;
		left: 0;
	}
	
	.lowin .lowin-box-inner {
		background-color: #fff;
		-webkit-box-shadow: 0 7px 25px rgba(0, 0, 0, .08);
		        box-shadow: 0 7px 25px rgba(0, 0, 0, .08);
		padding: 60px 25px 25px 25px;
		text-align: left;
		border-radius: 3px;
	}
	
	.lowin .lowin-box::after {
		content: ' ';
		-webkit-box-shadow: 0 0 25px rgba(0, 0, 0, .1);
		        box-shadow: 0 0 25px rgba(0, 0, 0, .1);
		-webkit-transform: translate(0, -92.6%) scale(.88);
		    -ms-transform: translate(0, -92.6%) scale(.88);
		        transform: translate(0, -92.6%) scale(.88);
		border-radius: 3px;
		position: absolute;
		top: 100%;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: #fff;
		z-index: -1;
	}
	
	.lowin .lowin-box.lowin-flip {
		-webkit-transform: rotate3d(0, 1, 0, -180deg);
		        transform: rotate3d(0, 1, 0, -180deg);
		display: none;
		opacity: 0;
	}
	
	.lowin .lowin-box p {
		color: var(--color-semidark);
		font-weight: 700;
		margin-bottom: 20px;
		text-align: center;
	}
	
	.lowin .lowin-box .lowin-group {
		margin-bottom: 20px;
	}
	
	.lowin .lowin-box label {
		margin-bottom: 5px;
		display: inline-block;
		width: 100%;
		color: var(--color-semidark);
		font-weight: 700;
	}
	
	.lowin .lowin-box label a {
		float: right;
	}
	
	.lowin .lowin-box .lowin-input {
		background-color: var(--color-grey);
		color: var(--color-dark);
		border: none;
		border-radius: 3px;
		padding: 15px 20px;
		width: 100%;
		outline: 0;
	}
	
	.lowin .lowin-box .lowin-btn {
		display: inline-block;
		width: 100%;
		border: none;
		color: #fff;
		padding: 15px;
		border-radius: 3px;
		background-color: var(--color-primary);
		-webkit-box-shadow: 0 2px 7px var(--color-semidark);
		        box-shadow: 0 2px 7px var(--color-semidark);
		font-weight: 700;
		outline: 0;
		cursor: pointer;
		-webkit-transition: all .5s;
		-o-transition: all .5s;
		transition: all .5s;
		margin-top: 10px;
	}
	
	.lowin .lowin-box .lowin-btn:active {
		-webkit-box-shadow: none;
		        box-shadow: none;
	}
	
	.lowin .lowin-box .lowin-btn:hover {
		opacity: .9;
	}
	
	.lowin .text-foot {
		text-align: center;
		padding: 10px;
		font-weight: 700;
		margin-top: 20px;
		color: var(--color-semidark);
	}
	
	.lowin .lowin-footer {
		text-align: center;
		margin: 30px 0;
		font-size: 12px;
		color: var(--color-semidark);
		font-weight: 700;
	}
	
	.lowin .login-back-link {
		-webkit-transition: all 1s;
		-o-transition: all 1s;
		transition: all 1s;
		display: none;
		opacity: 0;
	}
	
	/* animation */
	.lowin .lowin-box.lowin-animated {
		-webkit-animation-name: LowinAnimated;
		        animation-name: LowinAnimated;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
	}
	
	.lowin .lowin-box.lowin-animatedback {
		-webkit-animation-name: LowinAnimatedBack;
		        animation-name: LowinAnimatedBack;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
	}
	
	.lowin .lowin-box.lowin-animated-flip {
		-webkit-animation-name: LowinAnimatedFlip;
		        animation-name: LowinAnimatedFlip;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
	}
	
	.lowin .lowin-box.lowin-animated-flipback {
		-webkit-animation-name: LowinAnimatedFlipBack;
		        animation-name: LowinAnimatedFlipBack;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
	}
	
	.lowin .lowin-brand.lowin-animated {
		-webkit-animation-name: LowinBrandAnimated;
		        animation-name: LowinBrandAnimated;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
	}
	
	.lowin .lowin-group.password-group {
		-webkit-transition: all 1s;
		-o-transition: all 1s;
		transition: all 1s;
	}
	
	.lowin .lowin-group.password-group.lowin-animated {
		-webkit-animation-name: LowinPasswordAnimated;
		        animation-name: LowinPasswordAnimated;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
		-webkit-transform-origin: 0 0;
		    -ms-transform-origin: 0 0;
		        transform-origin: 0 0;
	}
	
	.lowin .lowin-group.password-group.lowin-animated-back {
		-webkit-animation-name: LowinPasswordAnimatedBack;
		        animation-name: LowinPasswordAnimatedBack;
		-webkit-animation-duration: 1s;
		        animation-duration: 1s;
		-webkit-animation-fill-mode: forwards;
		        animation-fill-mode: forwards;
		-webkit-animation-timing-function: ease-in-out;
		        animation-timing-function: ease-in-out;
		-webkit-transform-origin: 0 0;
		    -ms-transform-origin: 0 0;
		        transform-origin: 0 0;
	}
	
	/* keyframes */
	@-webkit-keyframes LowinAnimated {
		0% {
			-webkit-transform: rotate3d(0);
			        transform: rotate3d(0);
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 0;
			-webkit-transform: rotate3d(0, 1, 0, 180deg);
			        transform: rotate3d(0, 1, 0, 180deg);
		}
	}
	@keyframes LowinAnimated {
		0% {
			-webkit-transform: rotate3d(0);
			        transform: rotate3d(0);
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 0;
			-webkit-transform: rotate3d(0, 1, 0, 180deg);
			        transform: rotate3d(0, 1, 0, 180deg);
		}
	}
	
	@-webkit-keyframes LowinAnimatedBack {
		0% {
			opacity: 0;
			-webkit-transform: rotate3d(0, 1, 0, 180deg);
			        transform: rotate3d(0, 1, 0, 180deg);
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 1;
			-webkit-transform: rotate3d(0);
			        transform: rotate3d(0);
		}
	}
	
	@keyframes LowinAnimatedBack {
		0% {
			opacity: 0;
			-webkit-transform: rotate3d(0, 1, 0, 180deg);
			        transform: rotate3d(0, 1, 0, 180deg);
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 1;
			-webkit-transform: rotate3d(0);
			        transform: rotate3d(0);
		}
	}
	
	@-webkit-keyframes LowinAnimatedFlip {
		0% {
			-webkit-transform: rotate3d(0, 1, 0, -180deg);
			        transform: rotate3d(0, 1, 0, -180deg);
			opacity: 0;
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 1;
			-webkit-transform: rotate3d(0, 0, 0, 180deg);
			        transform: rotate3d(0, 0, 0, 180deg);
		}
	}
	
	@keyframes LowinAnimatedFlip {
		0% {
			-webkit-transform: rotate3d(0, 1, 0, -180deg);
			        transform: rotate3d(0, 1, 0, -180deg);
			opacity: 0;
		}
		99% {
			opacity: 1;
		}
		100% {
			opacity: 1;
			-webkit-transform: rotate3d(0, 0, 0, 180deg);
			        transform: rotate3d(0, 0, 0, 180deg);
		}
	}
	
	@-webkit-keyframes LowinAnimatedFlipBack {
		0% {
			opacity: 1;
			-webkit-transform: rotate3d(0, 0, 0, 180deg);
			        transform: rotate3d(0, 0, 0, 180deg);
		}
		95% {
			opacity: 0;
		}
		100% {
			-webkit-transform: rotate3d(0, 1, 0, -180deg);
			        transform: rotate3d(0, 1, 0, -180deg);
			opacity: 0;
		}
	}
	
	@keyframes LowinAnimatedFlipBack {
		0% {
			opacity: 1;
			-webkit-transform: rotate3d(0, 0, 0, 180deg);
			        transform: rotate3d(0, 0, 0, 180deg);
		}
		95% {
			opacity: 0;
		}
		100% {
			-webkit-transform: rotate3d(0, 1, 0, -180deg);
			        transform: rotate3d(0, 1, 0, -180deg);
			opacity: 0;
		}
	}
	
	@-webkit-keyframes LowinBrandAnimated {
		0% {
			-webkit-transform: translate(0, 0);
			        transform: translate(0, 0);
		}
		50% {
			-webkit-transform: translate(0, -120px);
			        transform: translate(0, -120px);
		}
		100% {
			-webkit-transform: translate(0, 0);
			        transform: translate(0, 0);
		}
	}
	
	@keyframes LowinBrandAnimated {
		0% {
			-webkit-transform: translate(0, 0);
			        transform: translate(0, 0);
		}
		50% {
			-webkit-transform: translate(0, -120px);
			        transform: translate(0, -120px);
		}
		100% {
			-webkit-transform: translate(0, 0);
			        transform: translate(0, 0);
		}
	}
	
	@-webkit-keyframes LowinPasswordAnimated {
		0% {
			-webkit-transform: rotate3d(0, 0, 0, 0);
			        transform: rotate3d(0, 0, 0, 0);
		}
		30% {
			opacity: 1;
		}
		60% {
			opacity: 0;
		}
		100% {
			opacity: 0;
			-webkit-transform: rotate3d(1, 0, 0, -180deg);
			        transform: rotate3d(1, 0, 0, -180deg);
			z-index: -1;
		}
	}
	
	@keyframes LowinPasswordAnimated {
		0% {
			-webkit-transform: rotate3d(0, 0, 0, 0);
			        transform: rotate3d(0, 0, 0, 0);
		}
		30% {
			opacity: 1;
		}
		60% {
			opacity: 0;
		}
		100% {
			opacity: 0;
			-webkit-transform: rotate3d(1, 0, 0, -180deg);
			        transform: rotate3d(1, 0, 0, -180deg);
			z-index: -1;
		}
	}
	
	@-webkit-keyframes LowinPasswordAnimatedBack {
		0% {
			opacity: 0;
			-webkit-transform: rotate3d(1, 0, 0, -180deg);
			        transform: rotate3d(1, 0, 0, -180deg);
		}
		40% {
			opacity: 0;
		}
		60% {
			opacity: 1;
		}
		100% {
			-webkit-transform: rotate3d(0, 0, 0, 0);
			        transform: rotate3d(0, 0, 0, 0);
		}
	}
	
	@keyframes LowinPasswordAnimatedBack {
		0% {
			opacity: 0;
			-webkit-transform: rotate3d(1, 0, 0, -180deg);
			        transform: rotate3d(1, 0, 0, -180deg);
		}
		40% {
			opacity: 0;
		}
		60% {
			opacity: 1;
		}
		100% {
			-webkit-transform: rotate3d(0, 0, 0, 0);
			        transform: rotate3d(0, 0, 0, 0);
		}
	}
	
	@media screen and (max-width: 320px) {
		.lowin .lowin-wrapper {
			width: 100%;
		}
		.lowin .lowin-box {
			padding: 0 10px;
		}
	}
</style>
