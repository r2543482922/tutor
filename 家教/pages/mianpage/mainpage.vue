<template>
	<view class="main">
		<view class="text">首页</view>
		<image class="banner" src="../../static/mainpage/banner.png" ></image>
		
		<view class="function">
			<view class="box">
				<view class="smallbox" @tap="student">
					<image  class="image" src="../../static/mainpage/icon-sy.png" ></image>
					<view class="font" >生源阁</view>
				</view>
			</view>
			<view class="box">
				<view class="smallbox" @tap="classlesson">
					<image  class="image" src="../../static/mainpage/icon-kb.png" ></image>
					<view class="font"	>课程表</view>
				</view>
			</view>
			<view class="box">
				<view class="smallbox" @tap="help">
					<image  class="image" src="../../static/mainpage/icon-hp.png" ></image>
					<view class="font" 	>帮助中心</view>
				</view>
			</view>
		</view>
		<view class="title-box">
			<view class="line"></view>
			<view class="title">我的课程</view>
			<view class="more">更多</view>
			<image class="arrow-right" src="../../static/mainpage/icon-arrow-right.png"></image>
		</view>
		<!--image class="crossing" src="../../static/mainpage/icon-fabu.png" ></image-->
		
		<view class="buttom-box">
			<view class="time">
			 <wCalendar ></wCalendar> 
			</view>
		</view>
		
		 <!-- <rwj-tabbar :current-page="0"></rwj-tabbar> -->
	</view>
	        
</template>

<script>
	import wCalendar from "@/components/uni-calendar/uni-calendar.vue";
	
	export default {
		components:{
		        wCalendar
		    },
			
		data() {
			return {
				
			}
		},
		 onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		        console.log(option.id); //打印出上个页面传递的参数。
		        console.log(option.name); //打印出上个页面传递的参数。
		    },
		
		methods: {
			student(){
				uni.navigateTo({
				    url: '../test/NULL'
				});
			},
			classlesson(){
				uni.navigateTo({
				    url: '../test/NULL'
				});
			},
			help(){
				uni.navigateTo({
				    url: '../test/NULL'
				});
			}
			
		}
	}
</script>

<style>
	@import url("./mainpage.css");
	
</style>

