<template>
	<view >
		<aki-particles  :particlestyle="particlestyle"></aki-particles>
			<u-notify ref="uNotify"></u-notify>
		<u-navbar	class='top'
		            leftText="退出"
		            title="称骨算法"
		            :safeAreaInsetTop="false"
					 @leftClick="leftClick"
					 rightText="❀☯❀☯❀"
					 :height='90'
					 bgColor="rgba(0,0,0,0)"
		        >
		
				</u-navbar>
				
		<rl class='pg'  @dt="giveme" @index="dx"></rl>
		<view class="tps" :style="height<=736||sb=='PC'?{left:lft,top:top}:''">
			<view>相</view>
			<view>信</view>
			<view>科</view>
			<view>学</view>
			<view>，</view>
			<view>切</view>
			<view>勿</view>
			<view>迷</view>
			<view>信</view>
			<view>!</view>
		</view>
		<view class="myinof" >
		</view>
	</view>
</template>

<script>
	import rl from '@/pages/render/render.vue'
	import akiparticles from '@/uni_modules/aki-particles/components/aki-particles/aki-particles.vue'//导入组件
	export default {
		components:{
			rl,
			"aki-particles":akiparticles
		},
		data() {
			return {
				num:0,
				height:'',
				lft:'87%',
				top:'42%',
				sb:'',
				particlestyle:{
					"style":"snow",
					"zindex":0,
					"bgcolor":"rgba(0,0,0,0)"
				},
			}
		},
		onLoad() {
		uni.getSystemInfo({
			success:(e)=> {
				this.height=e.screenHeight;
				this.sb=e.deviceModel;
				console.log(this.sb);
				if(e.deviceModel=='PC'||e.deviceModel=='iPad'){
					this.top='20%';
				}else{
					this.top='42%';
				}
			}
		})
		},
		onShow() {
		plus.navigator.setFullscreen(true);
			
			
		},
		methods: {
			giveme(e){
				console.log(e);
			},
	
			
			
			leftClick() {
				this.num+=1
				if(this.num%2==0){
					plus.runtime.quit();
				}else{
			            this.$refs.uNotify.show({
			                top: 0,
			                type: 'error',
			                color: '#ffffff',
			                bgColor: '#000000',
			                message: '再点击一次将会退出',
			                duration: 800,
			                fontSize: 20,
			                safeAreaInsetTop:true,
							
			            })
						}
			        }

		}
	}
</script>

<style>
.top{
	font-weight: bolder;
}
.pg{
	margin-top: 30px;
}
.tps{
	position: absolute;
	text-align: center;
	top: 42%;
	left: 48%;
	font-size: 34rpx;
	font-weight: bold;
}
.myinof{
	position: absolute;
	bottom: 0px;
}
</style>
