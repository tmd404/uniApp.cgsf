<template>
	<view>
		<view class="grace-btdialog-shade" @touchmove.stop.prevent="" v-if="show">
			<view class="pickerView dialog" v-show="isPicker">
				<view class="pickerHeaderTitle">
					<view class="close" @click="closeDialog">取消</view>
					<view class="actionBtn">
						<view :class="['solar', lunar === 'solar' ? 'current' : '']" @click="tabCalendar('solar')">公历</view>
						<view :class="['lunar', lunar === 'lunar' ? 'current' : '']" @click="tabCalendar('lunar')">农历</view>
					</view>
					<view class="confirm" @click="confirm">确定</view>
				</view>
				
				<picker-view :indicator-style="indicatorStyle" :value="value" @change="bindChange" style="width:750rpx; height: 400rpx;">
					<picker-view-column>
						<view class="item" v-for="(item,index) in years" :key="index">{{item}}年</view>
					</picker-view-column>
					<picker-view-column>
						<view class="item" v-for="(item,index) in months" :key="index">{{item}}月</view>
					</picker-view-column>
					<picker-view-column>
						<view class="item" v-for="(item,index) in days" :key="index">{{item}}日</view>
					</picker-view-column>
					<picker-view-column v-if="isHourShow">
						<view class="item" v-for="(item,index) in hours" :key="index">{{item}}</view>
					</picker-view-column>
					<picker-view-column v-if="isMinShow">
						<view class="item" v-for="(item,index) in mins" :key="index">{{item}}分</view>
					</picker-view-column>
				</picker-view>
			</view>
			<view class="confirmView dialog" v-show="!isPicker">
				<view class="pickerHeaderTitle" style="text-align: center; color: rgb(0, 0, 0);font-weight: bolder;font-size: 25px;">确认时间</view>
				<view style="background: #FFFFFF; padding: 20rpx; text-align: center;">
					<view class="confirmSubTitle">请确认输入的时间是否正确</view>
					<view class="solar"><text>公(阳)历：</text>{{solarDate}}</view>
					<view class="lunar"><text>农(阴)历：</text>{{lunarDate}}</view>
					<view class="buttons">
						<view class="blak" @click.stop="blak">返回修改</view>
						<view class="confirm" @click.stop="confirmDialog">确认提交</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import render from './calendar-data/render.js';
	let date = new Date();
	export default {
		name: "graceBottomDialog",
		props: {
			show : {
				type : Boolean,
				default : false
			},
			date : {
				type : String,
				default : date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate()
			},
			time : {
				type : String,
				default : ''
			},
			isHourShow : {
				type : Boolean,
				default : false
			},
			isMinShow : {
				type : Boolean,
				default : false
			},
			lunarType : {
				type : String,
				default : 'number' //number or words
			}
		},
		data() {
			return {
				years: [],
				months: [],
				days: [],
				year: '',
				month: '',
				day: '',
				
				hours: [],
				mins: [],
				hour: '',
				min: '',
				
				value: [],//当前选择的五类下标【可以是3类、3类】

				indicatorStyle: `height: ${Math.round(uni.getSystemInfoSync().screenWidth/(750/80))}px;`,
				
				lunar: 'solar',
				isPicker: true,
				solarDate: "",//国历最后时间
				lunarDate: "",//农历最终时间
				returnDate: "",//返回页面的日期
				returnTime: "",//返回页面的时间
			}
		},
		created:function(){
			this.init();
		},
		watch: {
			isHourShow(){
				this.init();
			},
			isMinShow(){
				this.init();
			},
			lunarType(){
				this.init();
			}
		},
		methods: {
			init: function() {
				date = new Date(this.date + ' ' + this.time.replace("-", ":"));
				this.year = date.getFullYear();
				this.month = date.getMonth() + 1;
				this.day = date.getDate();
				if(this.isHourShow) this.hour = date.getHours();
				if(this.isMinShow) this.min = date.getMinutes();
				
				let solarCalendar = render.init(this.lunar, 1900, 2100, this.year, this.month, this.day, this.hour, this.min, this.isHourShow, this.isMinShow, false, this.lunarType);
				
				this.years = solarCalendar.years;
				this.months = solarCalendar.months;
				this.days = solarCalendar.days;
				this.hours = solarCalendar.hours;
				this.mins = solarCalendar.mins;
				
				this.year = solarCalendar.years[solarCalendar.yearIndex];
				this.month = solarCalendar.months[solarCalendar.monthIndex];
				this.day = solarCalendar.days[solarCalendar.dayIndex];
				this.hour = solarCalendar.hourIndex;
				this.min = solarCalendar.minIndex;
				this.value = [solarCalendar.yearIndex, solarCalendar.monthIndex, solarCalendar.dayIndex, solarCalendar.hourIndex, solarCalendar.minIndex];
			},
			bindChange: function(e) {
				this.year = this.years[e.detail.value[0]];
				this.month = this.months[e.detail.value[1]];
				this.day = this.days[e.detail.value[2]];
				this.hour = e.detail.value[3];
				this.min = e.detail.value[4];
				
				//因为天数可能会变化
				let solarCalendar = render.init(this.lunar, 1900, 2100, this.year, this.month, this.day, this.hour, this.min, this.isHourShow, this.isMinShow, false, this.lunarType);
			
				this.years = solarCalendar.years;
				this.months = solarCalendar.months;
				this.days = solarCalendar.days;
				this.hours = solarCalendar.hours;
				this.mins = solarCalendar.mins;
			},
			tabCalendar: function (tab) {
				if(this.lunar === tab) return false;//同类不能重复转换
				
				this.lunar = tab;
				
				let solarCalendar = render.init(this.lunar, 1900, 2100, this.year, this.month, this.day, this.hour, this.min, this.isHourShow, this.isMinShow, true, this.lunarType);

				this.years = solarCalendar.years;
				this.year = solarCalendar.years[solarCalendar.yearIndex];
				this.months = solarCalendar.months;
				this.month = solarCalendar.months[solarCalendar.monthIndex];
				this.days = solarCalendar.days;
				this.day = solarCalendar.days[solarCalendar.dayIndex];
				this.hours = solarCalendar.hours;
				this.hour = solarCalendar.hourIndex;
				this.mins = solarCalendar.mins;
				this.min = solarCalendar.minIndex;
				this.value = [solarCalendar.yearIndex, solarCalendar.monthIndex, solarCalendar.dayIndex, solarCalendar.hourIndex, solarCalendar.minIndex];
			},
			confirm: function() {
				this.isPicker = false;
				
				if(this.lunar === 'solar') {
					this.solarDate = this.year + '年' + this.month + '月' + this.day + '日';
					this.returnDate = this.year + '-' + this.month + '-' + this.day;
					
					if(this.isHourShow === true) this.solarDate += this.hours[this.hour];
					if(this.isMinShow === true) this.solarDate += this.min + '分';
					
					let toValues = render.submit(this.lunar, this.year, this.month, this.day, this.hour, this.min, this.isHourShow, this.isMinShow);
					this.lunarDate = toValues.text;
					this.returnTime = toValues.hour + '-' + toValues.min;
				}else{
					this.lunarDate = this.year + '年' + this.month + '月' + this.day + '日';
					if(this.isHourShow === true) this.lunarDate += this.hours[this.hour];
					if(this.isMinShow === true) this.lunarDate += this.min + '分';
					
					let toValues = render.submit(this.lunar, this.year, this.month, this.day, this.hour, this.min, this.isHourShow, this.isMinShow);
					this.solarDate = toValues.text;
					this.returnDate = toValues.year + '-' + toValues.month + '-' + toValues.day;
					this.returnTime = toValues.hour + '-' + toValues.min;
				}
			},
			blak: function() {
				this.isPicker = true;
				this.lunarDate = "";
				this.solarDate = "";
			},
			closeDialog: function() {
				this.$emit('closeDialog');
			},
			confirmDialog: function() {
				this.$emit('confirmDialog', {date:this.returnDate,ndate:this.lunarDate, time:this.returnTime});
			}
		}
	}
</script>

<style>
.grace-btdialog-shade{position:fixed; width:100%; height:100%; left:0; top:0; z-index:9991; background:rgba(0, 0, 0, 0.5);}
.grace-btdialog-shade .dialog{width:100%; height:auto; background:#FFFFFF; position:absolute; bottom:0; left:0;}
.pickerHeaderTitle {
	background: #FFFFFF;
	border-bottom: 1rpx solid #E8E8E8;
	width: 100%;		/* 日期栏显示宽度 */
	height: 88rpx;
	line-height: 88rpx;
	padding: 0rpx 20rpx;
	font-size: 32rpx;
	display: flex;
	justify-content: space-around;
	align-items: center;
}
.pickerHeaderTitle .close {
	border: 1px solid black;
	height:65rpx;
	border-radius: 5px;
	width: 15%;
	text-align: center;
	line-height: 65rpx;
	color: rgb(0, 0, 0);  /* 取消按钮 */
	background-color: #ffffff;
	cursor: pointer;
}
.pickerHeaderTitle .actionBtn {
	display: flex;
	justify-content: center;
	align-items: center;
	
}
.pickerHeaderTitle .actionBtn .solar{
	height: 40rpx;
	line-height: 40rpx;
	padding: 10rpx 30rpx;
	border: 1rpx solid rgb(0, 0, 0);
	border-top-left-radius: 10rpx;
	border-bottom-left-radius: 10rpx;
	color: rgb(0, 0, 0);
}
.pickerHeaderTitle .actionBtn .lunar{
	height: 40rpx;
	line-height: 40rpx;
	padding: 10rpx 30rpx;
	border: 1rpx solid rgb(0, 0, 0);
	border-top-right-radius: 10rpx;
	border-bottom-right-radius: 10rpx;
	color: rgb(0, 0, 0);	/* 公历按钮文字颜色	 */
}
.pickerHeaderTitle .actionBtn .current {
	background: rgb(0, 0, 0);			/* 农历按钮背景色 */
	color: #FFFFFF;
	border: 1rpx solid black;
}


.pickerHeaderTitle .confirm  {
	border: 1px solid black;
	height: 65rpx;
	border-radius: 5px;
	width: 15%;
	text-align: center;
	line-height: 65rpx;
	background-color: #ffffff;		/* 确认按钮颜色 */
	color: rgb(0, 0, 0);
	cursor: pointer;
}
.item {
	line-height: 80rpx;
	text-align: center;
	font-size: 28rpx;
}

.confirmSubTitle {font-size: 34rpx; color: #000000; line-height: 48rpx; margin-top: 48rpx; padding-bottom: 20rpx;font-weight: bolder;}
.confirmView .solar, .confirmView .lunar {font-size: 34rpx; color: rgb(0, 0, 0); line-height: 34rpx; padding: 10rpx 0rpx;font-weight: bolder;}
.confirmView .buttons {padding: 40rpx 0rpx; display: flex; flex-direction: row; justify-content: center; color: #FFFFFF; font-size: 34rpx;}
.confirmView .buttons .blak {display: inline-block; padding: 20rpx 40rpx; border-radius: 10rpx; background: #000000;}
.confirmView .buttons .confirm {display: inline-block; padding: 20rpx 40rpx; margin-left: 40rpx; border-radius: 10rpx; background: rgb(0, 0, 0);}
</style>
