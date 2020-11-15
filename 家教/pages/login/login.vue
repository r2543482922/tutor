<template>
	<view class="main">	
		<view class="login-top" style="background-image: url(../../static/login/background.png);background-repeat: no-repeat;
		background-size: 375px 667px;">
			<view class="mainHead"></view>
			<view class="logintop">
				<view  @click="regiser">注册</view>
			</view>	
			<view class="hello">您好!</view>
			<view class="welcom">欢迎您的到来～</view>
		</view>
		
		<view class="login">
			<view class="column">
				<view class="row" >
					<view class="password">密码登录</view>
					<view class="equal">验证码登录</view>	
				</view>				
					<image class="line" src="../../static/login/line.png"></image>
					<image class="line-2" src="../../static/login/line.png" ></image>
			</view>
			<view class="kuang1" style="background-image: url(../../static/login/kuang.png); background-repeat: no-repeat;
			background-size: 285px 40px;">
				<input type="text" class="input" v-model="UserName" placeholder="请输入手机号码/用户名" >
			</view>
					
			<view class="kuang2" style="background-image: url(../../static/login/kuang.png); background-repeat: no-repeat;
			background-size: 285px 40px;">
				<input type="text" class="input" v-model="PassWord" :password="showPassword"  placeholder="请输入登录密码" >
				<image class="icon-eye"  @click="Switch()"  :src="showPassword ? closeeye:openeye"  ></image>
			</view>
			<button  type="primary" class="button" hover-class="none" @click="login">登录</button>
		</view>
		
		<view class="forget" @click="forget">忘记密码?</view>          
	</view>	
</template>

<script>
	export default {
		data() {
			return {
				UserName:''	,		//账户
				PassWord:''	,		//密码
				showPassword: true,
				openeye:require('../../static/login/icon-eye2.png'),
				closeeye:require('../../static/login/icon-eye1.png')
			}
		},
		
		onShow() {
		  	uni.getStorage({
		  		key:'UserInfo',
				success:(e)=>{
					this.UserName = e.data.UserName  
					this.PassWord = e.data.PassWord
				}
		  	})
		},
		
		
		
		methods: {
			Switch(){
				this.showPassword = !this.showPassword
			},
			
			login(e){
				console.log(this.PassWord)
				console.log(this.UserName)
				var that = this
				if(that.UserName && that.PassWord){
					if(that.UserName =="admin" && that.PassWord == "123"){
						/* 验证成功跳转目标页面 */
						uni.reLaunch({
							url:'../mianpage/mainpage'
						})
					}
					else{
						uni.showToast({
							title: '密码或账户错误'
						});
					}
				}
				else{
						uni.showToast({
							title: '密码或账户为空'
						});
				}
			},
			
			regiser(){
				uni.reLaunch({
					url:'../mianpage/mainpage'
				})
			},
			
			forget(){
				uni.reLaunch({
					url:'../register/index'
				})
			}
			
		}
	}
</script>

<style>
	@import url("./login.css");
	
</style>
