<template>
	<view class="uni-calendar" >
			<view class="uni-calendar__header">
				<view class="uni-calendar__header-btn-box" @click="pre">
					<view class="uni-calendar__header-btn uni-calendar--left"></view>
				</view>
				<text class="uni-calendar__header-text">{{ (nowDate.year||'') +'年'+( nowDate.month||'') +'月'}}</text>
				<view class="uni-calendar__header-btn-box" @click="next">
					<view class="uni-calendar__header-btn uni-calendar--right"></view>
				</view>
				<text class="uni-calendar__backtoday" @click="backtoday">返回今天</text>
			</view>
			<view class="uni-calendar__box">
				
				<view class="uni-calendar__weeks">
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">日</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">一</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">二</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">三</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">四</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">五</text>
					</view>
					<view class="uni-calendar__weeks-day">
						<text class="uni-calendar__weeks-day-text">六</text>
					</view>
				</view>
				<view class="uni-calendar__weeks" v-for="(item,weekIndex) in weeks" :key="weekIndex">
					<view class="uni-calendar__weeks-item" v-for="(weeks,weeksIndex) in item" :key="weeksIndex">
						<uni-calendar-item :weeks="weeks" :calendar="calendar" :selected="selected"  @change="choiceDate"></uni-calendar-item>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import Calendar from './util.js';
	import uniCalendarItem from './uni-calendar-item.vue'
	/**
	 * Calendar 日历
	 * @description 日历组件可以查看日期，选择任意范围内的日期，打点操作。常用场景如：酒店日期预订、火车机票选择购买日期、上下班打卡等
	 * @tutorial https://ext.dcloud.net.cn/plugin?id=56
	 * @property {String} date 自定义当前时间，默认为今天
	 * @property {Boolean} lunar 显示农历
	 * @property {String} startDate 日期选择范围-开始日期
	 * @property {String} endDate 日期选择范围-结束日期
	 * @property {Boolean} range 范围选择
	 * @property {Boolean} insert = [true|false] 插入模式,默认为false
	 * 	@value true 弹窗模式
	 * 	@value false 插入模式
	 * @property {Array} selected 打点，期待格式[{date: '2019-06-27', info: '签到', data: { custom: '自定义信息', name: '自定义消息头',xxx:xxx... }}]
	 * @property {Boolean} showMonth 是否选择月份为背景
	 * @event {Function} change 日期改变，`insert :ture` 时生效
	 * @event {Function} confirm 确认选择`insert :false` 时生效
	 * @event {Function} monthSwitch 切换月份时触发
	 * @example <uni-calendar :insert="true":lunar="true" :start-date="'2019-3-2'":end-date="'2019-5-20'"@change="change" />
	 */
	export default {
		components: {
			uniCalendarItem
		},
		props: {
			date: {
				type: String,
				default: ''
			},
			selected: {
				type: Array,
				default () {
					return []
				}
			},
			lunar: {
				type: Boolean,
				default: false
			},
			startDate: {
				type: String,
				default: ''
			},
			endDate: {
				type: String,
				default: ''
			},
			range: {
				type: Boolean,
				default: false
			},
			insert: {
				type: Boolean,
				default: true
			},
			showMonth: {
				type: Boolean,
				default: true
			}
		},
		data() {
			return {
				show: false,
				weeks: [],
				calendar: {},
				nowDate: '',
				aniMaskShow: false
			}
		},
		watch: {
			selected(newVal) {
				this.cale.setSelectInfo(this.nowDate.fullDate, newVal)
				this.weeks = this.cale.weeks
			}
		},
		created() {
			// 获取日历方法实例
			this.cale = new Calendar({
				date: this.date,
				selected: this.selected,
				startDate: this.startDate,
				endDate: this.endDate,
				range: this.range,
			})
			this.init(this.cale.date.fullDate)
		},
		methods: {
			// 取消穿透
			clean() {},
			init(date) {
				this.weeks = this.cale.weeks
				this.nowDate = this.calendar = this.cale.getInfo(date)
			},
			open() {
				this.show = true
				this.$nextTick(() => {
					setTimeout(() => {
						this.aniMaskShow = true
					}, 50)
				})
			},
			close() {
				this.aniMaskShow = false
				this.$nextTick(() => {
					setTimeout(() => {
						this.show = false
					}, 300)
				})
			},
			confirm() {
				this.setEmit('confirm')
				this.close()
			},
			change() {
				if (!this.insert) return
				this.setEmit('change')
			},
			monthSwitch() {
				let {
					year,
					month
				} = this.nowDate
				this.$emit('monthSwitch', {
					year,
					month: Number(month)
				})
			},
			setEmit(name) {
				let {
					year,
					month,
					date,
					fullDate,
					lunar,
					extraInfo
				} = this.calendar
				this.$emit(name, {
					range: this.cale.multipleStatus,
					year,
					month,
					date,
					fulldate: fullDate,
					lunar,
					extraInfo: extraInfo || {}
				})
			},
			choiceDate(weeks) {
				if (weeks.disable) return
				this.calendar = weeks
				// 设置多选
				this.cale.setMultiple(this.calendar.fullDate)
				this.weeks = this.cale.weeks
				this.change()
			},
			backtoday() {
				this.cale.setDate(this.date)
				this.weeks = this.cale.weeks
				this.nowDate = this.calendar = this.cale.getInfo(this.date)
				this.change()
			},
			pre() {
				const preDate = this.cale.getDate(this.nowDate.fullDate, -1, 'month').fullDate
				this.setDate(preDate)
				this.monthSwitch()

			},
			next() {
				const nextDate = this.cale.getDate(this.nowDate.fullDate, +1, 'month').fullDate
				this.setDate(nextDate)
				this.monthSwitch()
			},
			setDate(date) {
				this.cale.setDate(date)
				this.weeks = this.cale.weeks
				this.nowDate = this.cale.getInfo(date)
			}
		}
	}
</script>

<style scoped>
	.uni-calendar {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		width: 351px;
		height: 213px;
	}

	

	.uni-calendar__content {
		background-color: #fff;
		
	}

	.uni-calendar__header {
		position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		justify-content: center;
		align-items: center;
		margin-top: 14px;
		height: 15px;
		border-bottom-color: #e5e5e5;
	}

	
	.uni-calendar__backtoday {
		position: absolute;
		right: 0;
		padding: 0 5px;
		padding-left: 10px;
		height: 25px;
		line-height: 25px;
		font-size: 12px;
		border-top-left-radius: 25px;
		border-bottom-left-radius: 25px;
		color: #eb9229;
		
	}

	.uni-calendar__header-text {
		text-align: center;
		width: 100px;
		font-size: 15px;
		color: #333333;
	}

	.uni-calendar__header-btn-box {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		width: 50px;
		height: 50px;
	}

	.uni-calendar__header-btn {
		width: 10px;
		height: 10px;
		border-left-color: #808080;
		border-left-style: solid;
		border-left-width: 2px;
		border-top-color: #555555;
		border-top-style: solid;
		border-top-width: 2px;
	}

	.uni-calendar--left {
		transform: rotate(-45deg);
	}

	.uni-calendar--right {
		transform: rotate(135deg);
	}


	.uni-calendar__weeks {
		position: relative;
		display: flex;
		flex-direction: row;
		height: 10px;
		margin-top: 15px;
	}

	.uni-calendar__weeks-item {
		flex: 1;
	}

	.uni-calendar__weeks-day {
		flex: 1;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 18px;
		border-bottom-color: #F5F5F5;
	}

	.uni-calendar__weeks-day-text {
		font-size: 12px;
	}

	.uni-calendar__box {
		position: relative;
		width: 351px;
		height: 213px;
	}

</style>