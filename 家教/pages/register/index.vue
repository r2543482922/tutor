<template>
	<view>
		<view style="padding-top:260rpx;">
			<!--view style="height:80px;">
				<view class="UserName-input" :class="{active:distinguish.Phone==true}">
					<image src="../../static/yx-login/phone.png"></image>
					<input type="text" placeholder="请输入您的手机号码" placeholder-style="color:#fff" v-model="From.Phone"/>
					<image src="../../static/yx-login/delete.png" class="delete" v-if="From.Phone" @click="From.Phone=null" style="width:22px;"></image>
					<view class="obtain" @click="countdown">{{time}}</view>
				</view>
				<text class="tips" v-if="Phonetips">{{Phonetips}}</text>
			</view-->
			
			<view style="height:80px;">
			<view class="login-input" :class="{active:distinguish.UserName==true}">
				<image src="../../static/yx-login/user.png"></image>
				<input type="text" placeholder="请输入您的用户名" placeholder-style="color:#fff" v-model="From.UserName"/>
				<image src="../../static/yx-login/delete.png" class="delete" v-if="From.UserName" @click="From.UserName=null"></image>
			</view>
			<text class="tips" v-if="UserNametips">{{UserNametips}}</text>
			</view>
			
			<view style="height:80px;">
			<view class="login-input" :class="{active:distinguish.PassWord==true}">
				<image src="../../static/yx-login/password.png"></image>
				<input type="password" placeholder="请输入您要注册的密码" placeholder-style="color:#fff" v-model="From.PassWord" v-if="SwiTch"/>
				<input type="text" placeholder="请输入您要注册的密码" placeholder-style="color:#fff" v-model="From.PassWord" v-else/>
				<image src="../../static/yx-login/delete.png" class="delete" v-if="From.PassWord" @click="From.PassWord=null" style="padding-right:10px;width:22px;"></image>
				<image src="../../static/yx-login/yanjing.png" class="switch" @click="Switch()"></image>
			</view>
			<text class="tips" v-if="PassWordtips">{{PassWordtips}}</text>
			</view>
		</view>
		<view class="login-button">
			<button @click="register">点击注册</button>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				From:{
					Phone:'',
					UserName:'',
					PassWord:''
				},
				distinguish:{
					Phone:false,
					UserName:false,
					PassWord:false
				},
				Phonetips:'',
				UserNametips:'',
				PassWordtips:'',
				time:'点击获取',
				SwiTch:true
			}
		},
		methods:{
			countdown(){
				var that = this
				that.time = 4
				if(that.time>0){
				 var interval = setInterval(function(){
					that.time--
					if(that.time == 0){
						clearInterval(interval)
						that.time = "重新获取"
					}
				   },1000)  
				}
			},
			register(){
				var Phone = /^((13[0-9])|(14[5|7])|(15([0-3]|[5-9]))|(18[0,5-9]))\d{8}$/;
				//用户名正则，4到16位（字母，数字，下划线，减号）
				var UserName = /^[a-zA-Z0-9_-]{4,16}$/;
				//密码强度正则，最少6位，包括至少1个大写字母，1个小写字母，1个数字，1个特殊字符
				var PassWord = /^.*(?=.{6,})(?=.*\d)(?=.*[A-Z])(?=.*[a-z])(?=.*[!@#$%^&*? ]).*$/;
				//验证电话
				if(this.From.Phone){
					if(!Phone.test(this.From.Phone)){
						this.Phonetips = '手机号格式错误'	
					}else{
						this.distinguish.Phone = false
						this.Phonetips = ""
					}
				}else{
					this.distinguish.Phone = true
					this.Phonetips = '手机号不能为空'
				}
				
				//验证用户名
				if(this.From.UserName){	
					if(!UserName.test(this.From.UserName)){
						this.UserNametips = '用户名格式错误'
					}else{
						this.distinguish.UserName = false
						this.UserNametips = ""
					}
				}else{
					this.distinguish.UserName = true
					this.UserNametips = '用户名不能为空'
				}
				
				// 验证密码
				if(this.From.PassWord){	
					if(!PassWord.test(this.From.PassWord)){
						this.PassWordtips = '密码格式错误'
					}else{
						this.distinguish.PassWord = false
						this.PassWordtips = ""
					}
				}else{
					this.distinguish.PassWord = true
					this.PassWordtips = '密码不能为空'
				}
			},
			Switch(){
				this.SwiTch = !this.SwiTch
			}
		}
	}
</script>

<style lang="css">
	input:focus { outline: none; }
	/* 背景图设置 */
	page{
		width:100%;
		height:100%;
		background-color: #007AFF;
		background-repeat: no-repeat;
		background-size:100% 100%;
		}
	.login-input{
		width:85%;
		height:63px;
		margin:0px auto;
		border-bottom:1px solid   rgb(255,255,255);
		display: flex;
		}

	
		input{
			width:91%;
			border: none;
			height:40px;
			color: rgb(255,255,255);
			padding-left:15px;
			padding-right:15px;
			font-size:14px;
			margin-top:25px;
		}
		image{
			width:23px;
			height:20px;
			margin-top:35px;
		}
		.delete{
			width:20px;
			height:20px;
			margin-left:-20px;
		}
		.switch{
			width:23px;
			height:14px;
			padding-top:3px;
		}
	
	.UserName-input{
		width:85%;
		height:63px;
		margin:0px auto;
		border-bottom:1px solid rgb(255,255,255);
		display: flex;
		}
		input{
			width:65%;
			border: none;
			height:40px;
			color: rgb(255,255,255);
			padding-left:15px;
			padding-right:15px;
			font-size:14px;
			margin-top:25px;
		}
		image{
			width:23px;
			height:20px;
			margin-top:35px;
		}
		button{
			margin-top:30px;
			margin-left:6px;
			border-radius:1px;
		}
		.delete{
			width:20px;
			height:20px;
			margin-left:-20px;
		}
		.obtain{
			width:90px;
			height:33px;
			font-size:13px;
			color: rgb(255,255,255);
			text-align: center;
			line-height:33px;
			padding-top:29px;
		}
	
	.login-button{
		width:85%;
		margin:0px auto;
		}
		
		button{
			width:100%;
			font-size:15px;
			height:40px;
			color: rgb(255,255,255);
			background-color: rgb(78,110,242);
			margin-top:16px;
		}
	
	.active{
		color:rgb(245,8,8);
		border-bottom:1px solid rgb(245,8,8);
	}
	.tips{
	   	color:rgb(245,8,8);
		font-size:12px;
		display: initial;
		float: right;
		padding-right:30px;
	}
</style>
