<template>
	<view >
		
		<view style="padding-top:40px ;" class="btn" >
			<image  class="start" mode="heightFix" src="../../static/lp.png" @click="showDateDialog('number')"></image>
		<view class="lh"><image  src="../../static/hehua.png" @click="showDateDialog('number')"></image></view>
		</view>
		<zan-calendar 
			:date="date"
			:time="time" 
			:isHourShow="isHourShow" 
			:isMinShow="isMinShow" 
			:show="dateDialog" 
			:lunarType = "lunarType"
			@closeDialog="closeDialog" 
			@confirmDialog="confirmDialog"
		>
		</zan-calendar>
		<u-steps class='lft' activeColor="#000" :current="cur" direction="column">
				<u-steps-item title="点击八卦图" desc="或点击莲花">
				</u-steps-item>
				<u-steps-item title="选择出生日期" desc="农历或公历">
				</u-steps-item>
			</u-steps>
	</view>
</template>

<script>
	import zanCalendar from '@/components/quick-calendar/calendar';
	export default {
		data() {
			return {
				     
				cur:0,//步骤
				date: '2000-01-22',//日期
				time: '13-26',//时间
				isHourShow: false,//是否显示时辰（小时）
				isMinShow: false,//是否显示分钟
				
				lunarType: 'number', //年月日显示方式
				
				dateDialog: false, //是否弹出日历组件
				mdate:'',	//传递日期
				mtime:'',	//传递时间
				mndate:''	//传递农历日期
			}
		},
		//注册主键（参考vue文档）
		components:{
			zanCalendar
		},
		
		//操作方法（参考vue文档）
		methods: {
			
			//示例，展示三种不同的选择调度
			showDateDialog: function(lunarType) {
						this.isHourShow = true;
						this.isMinShow = true;
				this.dateDialog = true;
				this.lunarType = lunarType;
				this.cur=1;
			},
			//示例，在为确认是就点击了取消，直接关闭弹窗
			closeDialog: function() {
				this.dateDialog = false;
				this.cur=0;
			},
			//示例，点击了确认后的相关操作，并再次点击确认时间后的返回，这里可以写自己的操作了
			confirmDialog: function(e) {
			this.cur=2;
			this.mdate=e.date;
			this.mtime=e.time;
			this.mndate=e.ndate;
			this.dateDialog = false;
			let lst={
			date:this.mdate,
			time:this.mtime,
			ndate:this.mndate
			}
			uni.navigateTo({
				url:`/pages/shower/shower?date=${JSON.stringify(lst)}`
			})
			},

	
	},
	}
</script>

<style>
	.lft{
		position: absolute;
		top:50%;
		font-weight: bolder;
	}
.btn{text-align: center;}
.start{
	margin-top:20px;
	
	animation: fadenum 100s linear infinite;
}
   @keyframes fadenum{
   100%{transform:rotate(360deg);
   }}
image{
	height: 250px;
}
.lh{
	width: 100%;
	position: absolute;
	bottom: 0px;
}
   
</style>
