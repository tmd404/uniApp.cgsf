<template>
	
	<view>
		<aki-particles :particlestyle="particlestyle"></aki-particles>
		<view>
		<u-navbar
		class="top"
		leftText="返回主页"
		title="称骨算法"
		:safeAreaInsetTop="false"
		@leftClick="leftClick"
		rightText="❀☯❀☯❀"
		:height='90'
		:fixed="false"
		bgColor="rgba(0,0,0,0)"
		>
		</u-navbar>
		</view>
				
				<view class="sktop">
				<u-divider 
						class="fg"
				        text="生辰时刻"
						textSize="18"
						:hairline="false"
				        textColor="#000000"
				        lineColor="#000000"></u-divider>
				</view>
			<view>
				<u-cell-group>
						<u-cell icon="calendar-fill" :title="date" value="公历日期"></u-cell>
						<u-cell icon="calendar" :title="ndate" value="农历日期"></u-cell>
					</u-cell-group>
			</view>
			<view class="tips">提示：闰月上半月算作本月，下半月算作下月(如：闰3月15算作3月，闰3月16算作4月)</view>
			<view >
			<u-divider 
					class="ndtp"
			        text="选择性别"
					textSize="18"
					:hairline="false"
			        textColor="#000000"
			        lineColor="#000000"></u-divider>
			</view>
			
			<view>
							<u-grid
							                :border="false"
							                @click="click"
											col="2"
											
							        >
							            <u-grid-item
							                    v-for="(baseListItem,baseListIndex) in baseList"
							                    :key="baseListIndex"
							            >
							                <u-icon
							                        :customStyle="{paddingTop:20+'rpx'}"
							                        :name="baseListItem.name"
							                        :size="22"
							                ></u-icon>
							                <text class="grid-text">{{baseListItem.title}}</text>
							            </u-grid-item>
							        </u-grid>
									 <u-toast type="default" ref="uToast" />
									 <u-cell-group style='margin-top: 10px;'>
									 		<u-cell :icon="sexico" :title="xb" :value="xbms"></u-cell>
									 	</u-cell-group>
			</view>
			
			
			<view class="img">
				<image @click="cgimg" mode="heightFix" :src="imgs.tfsc"></image>
			</view>
			
			
			
			<view class="context">
			<view>
			<u-divider 
					class="fg"
			        text="骨重命理"
					textSize="18"
					:hairline="false"
			        textColor="#000000"
			        lineColor="#000000"></u-divider>
			</view>
			<view class="info">
				<view>年份骨重:{{mynf}}两</view>
				<view>月份骨重:{{myyf}}两</view>
				<view>日份骨重:{{myrf}}两</view>
				<view>时刻骨重:{{mysk}}两</view>
				<view>总骨重:{{myz}}</view>
			</view>
			<view   @click="gzml" style="margin-top: 10px;">
			 <u-collapse
			    accordion
			        >
			 <u-collapse-item

			    :title="ml">
			                <u-icon :name="sexico" size="20" slot="icon"></u-icon>
			                <text class="u-collapse-content">称骨详解：{{xj}}</text>
			            </u-collapse-item>
						 </u-collapse>
						 </view>
			</view>
			
				
	</view>
</template>

<script>
	import msj from '@/components/mjson.js'
	import womsj from '@/components/womjson.js'
	import akiparticles from '@/uni_modules/aki-particles/components/aki-particles/aki-particles.vue'//导入组件
	
	export default {
		components:{
			"aki-particles":akiparticles
		},
		data() {
			return {
				mynf:'',
				myyf:'',
				myrf:'',
				mysk:'',
				mmyz:'',
				wommyz:'',
				myz:'',
				imgs:{
					tfnum:1,
					v1:'../../static/ds01.png',
					v2:'../../static/ds03.png',
					tfsc:'../../static/ds01.png'
				},
				particlestyle:{
					"style":"default",
					"zindex":0,
					"bgcolor":"rgba(0,0,0,0)"
				},
				mjson:msj,
				womjson:womsj,
				 baseList: [
				                    {
				                        name: 'man',
				                        title: '男'
				                    },
				                    {
				                        name: 'woman',
				                        title: '女'
				                    },
				                ],
				
				sexico:'man',
				xb:' 男',
				xbms:'当前命理：男命',
				ml:'',
				xj:'',
				ml1:'',
				xj1:'',
				ml2:'',
				xj2:'',
				date:'',
				ndate:'',
			years:{
			wu:[1972,2032,1929,1989,1967,2027,1944,2004,1956,2016,1969,2029],
			liu:[1926,1986,1941,2001,1917,1977,1955,2015,1919,1979,1946,2006,1947,2007,1923,1983],
			qi:[1960,2020,1949,2009,1961,2021,1973,2033,1927,1987,1953,2013,1965,2025,1943,2003,1932,1992],
			ba:[1937,1997,1938,1998,1975,2023,1964,2024,1976,2036,1942,2002,1931,1991,1920,1980,1933,1993],
			jiu:[1925,1985,1950,2010,1962,2022,1930,1990,1970,2030,1935,1995,1959,2019],
				
			yi:[1852,2012,1922,1982],
			yier:[1924,1984,1974,2034,1951,2011,1963,2023,1928,1988,1940,2000],
			yisan:[1966,2026],
			yisi:[1968,2028,1957,2017,1958,2018],
			yiwu:[1948,2008,1954,2014,1945,2005,1934,1994],
			yiliu:[1936,1996,1921,1981,1947,2007],
			yiqi:[1971,2031],
			yijiu:[1937,1999,1978,2038],
			
			},
			
		}
			
		},
		onShow() {
			plus.navigator.setFullscreen(true);
			
		},
		onLoad(option) {
			setTimeout(()=>{
								 uni.pageScrollTo({
								 	scrollTop:9999999,
								 	duration:500		
								 })
			},500)
			try{
				var yweight=0;
				var mweight=0;
				var dweight=0;
				var sweight=0;
				var contweight=0;
				var years=this.years;
				var mjso=this.mjson.data;
				var womjso=this.womjson.data;
				let dat=JSON.parse(option.date);
				if(JSON.parse(option.date)){
					let date=dat.date; //公历日期
					let time=dat.time;	//时间
					let ndate=dat.ndate; //农历时间
					
					let mdate1=date.split('-')[0];
					let mdate2=date.split('-')[1]<10?'0'+date.split('-')[1]:date.split('-')[1];
					let mdate3=date.split('-')[2]<10?'0'+date.split('-')[2]:date.split('-')[2];
					
					let mtime1=time.split('-')[0];
					let mtime2=time.split('-')[1];
					
					let mndate1=ndate.split('年')[0];
					let mnd=ndate.split('月')[0].split('年')[1];
					
					let mndate3=ndate.split('日')[0].split('月')[1]<10?'0'+ndate.split('日')[0].split('月')[1]:ndate.split('日')[0].split('月')[1];
					if(mnd.includes('闰')){
						if(mndate3<=15){
							let reg = /[\u4e00-\u9fa5]+/g;
							let sk = mnd.replace(reg,"");
							var mndate2=sk<10?'0'+sk:sk;
						console.log(mndate2);
					}else{
						let reg = /[\u4e00-\u9fa5]+/g;
						let sk = mnd.replace(reg,"");
						var mndate2=sk<10?'0'+parseInt(sk*1+1):sk+parseInt(sk*1+1);
					}
					}else{
					var mndate2=mnd<10?'0'+ndate.split('月')[0].split('年')[1]:ndate.split('月')[0].split('年')[1];
					}
					let mndate4=ndate.split('日')[1].split('分');
	
					this.date=`公历 ${mdate1}年 ${mdate2}月 ${mdate3}日 ${mtime1}点 ${mtime2}分`;
					this.ndate=`农历 ${mndate1}年 ${mndate2}月 ${mndate3}日 ${mtime1}点 ${mtime2}分`;
					
					let reg = /[0-9]+/g;
					let sk = mndate4[0].replace(reg,"");
					
					console.log('年：'+mndate1);
					console.log('月：'+mndate2);
					console.log('日：'+mndate3);
					console.log('时：'+sk);
				for(var w1=0;w1<years.wu.length;w1++){
					if(mndate1==years.wu[w1]){
						yweight=0.5;
					}
				}
				
				for(var w2=0;w2<years.liu.length;w2++){
					if(mndate1==years.liu[w2]){
						yweight=0.6;
					}
				}
				
				for(var w3=0;w3<years.qi.length;w3++){
					if(mndate1==years.qi[w3]){
						yweight=0.7;
					}
				}
				
				for(var w4=0;w4<years.ba.length;w4++){
					if(mndate1==years.ba[w4]){
						yweight=0.8;
					}
				}
				
				for(var w5=0;w5<years.jiu.length;w5++){
					if(mndate1==years.jiu[w5]){
						yweight=0.9;
					}
				}
					
				for(var x1=0;x1<years.yi.length;x1++){
					if(mndate1==years.yi[x1]){
						yweight=1.0;
					}
				}
				
				for(var x2=0;x2<years.yier.length;x2++){
					if(mndate1==years.yier[x2]){
						yweight=1.2;
					}
				}
				
				for(var x3=0;x3<years.yisan.length;x3++){
					if(mndate1==years.yisan[x3]){
						yweight=1.3;
					}
				}
				
				for(var x4=0;x4<years.yisi.length;x4++){
					if(mndate1==years.yisi[x4]){
						yweight=1.4;
					}
				}
				
				for(var x5=0;x5<years.yiwu.length;x5++){
					if(mndate1==years.yiwu[x5]){
						yweight=1.5;
					}
				}
				
				for(var x6=0;x6<years.yiliu.length;x6++){
					if(mndate1==years.yiliu[x6]){
						yweight=1.6;
					}
				}
				
				for(var x7=0;x7<years.yiqi.length;x7++){
					if(mndate1==years.yiqi[x7]){
						yweight=1.7;
					}
				}
				
				for(var x8=0;x8<years.yijiu.length;x8++){
					if(mndate1==years.yijiu[x8]){
						yweight=1.9;
					}
				}
				
				if(mndate2=='01'){
					mweight=0.6;
				}
				else if(mndate2=="02"){
					mweight=0.7;
				}
				else if(mndate2=="03"){
					mweight=1.8;
				}
				else if(mndate2=="04"){
					mweight=0.9;
				}
				else if(mndate2=="05"){
					mweight=0.5;
				}
				else if(mndate2=="06"){
					mweight=1.6;
				}
				else if(mndate2=="07"){
					mweight=0.9;
				}
				else if(mndate2=="08"){
					mweight=1.5;
				}
				else if(mndate2=="09"){
					mweight=1.8;
				}
				else if(mndate2=="10"){
					mweight=0.8;
				}
				else if(mndate2=="11"){
					mweight=0.9;
				}
				else if(mndate2=="12"){
					mweight=0.5;
				}
				
				if(mndate3=="01"||mndate3=="19"){
					dweight=0.5;
				}
				if(mndate3=="30"){
					dweight=0.6;
				}
				if(mndate3=="27"){
					dweight=0.7;
				}
				if(mndate3=="03"||mndate3=="07"||mndate3=="09"||mndate3=="13"||mndate3=="16"||mndate3=="23"||mndate3=="28"){
					dweight=0.8;
				}
				if(mndate3=="11"||mndate3=="17"||mndate3=="22"||mndate3=="24"){
					dweight=0.9;
				}
				if(mndate3=="02"||mndate3=="15"||mndate3=="20"||mndate3=="21"){
					dweight=1;
				}
				if(mndate3=="04"||mndate3=="06"||mndate3=="25"){
					dweight=1.5;
				}
				if(mndate3=="05"||mndate3=="08"||mndate3=="10"||mndate3=="29"){
					dweight=1.6;
				}
				if(mndate3=="12"||mndate3=="14"){
					dweight=1.7;
				}
				if(mndate3=="18"||mndate3=="26"){
					dweight=1.8;
				}
				
				if(sk=="丑"||sk=="戌"||sk=="亥"){
					sweight=0.6;
				}
				if(sk=="寅"){
					sweight=0.7;
				}
				if(sk=="未"||sk=="申"){
					sweight=0.8;
				}
				if(sk=="辰"||sk=="酉"){
					sweight=0.9;
				}
				if(sk=="卯"||sk=="午"){
					sweight=1;
				}
				if(sk=="子"||sk=="巳"){
					sweight=1.6;
				}
				
			console.log('年重：'+yweight);	
			console.log('月重：'+mweight);
			console.log('日重：'+dweight);
			console.log('时重：'+sweight);
			contweight=(yweight+mweight+dweight+sweight).toFixed(1);
			console.log('总骨重：'+contweight);
			this.mynf=yweight;
			this.myyf=mweight;
			this.myrf=dweight;
			this.mysk=sweight;
			
			for(var jso=0;jso<mjso.length;jso++){
				if(mjso[jso].id==contweight){
					console.log('男命：'+mjso[jso].title);
					this.xj1=mjso[jso].content;
					this.ml1=mjso[jso].t1+'男命称骨歌：'+mjso[jso].title;
					this.ml=this.ml1;
					this.xj=this.xj1;
					this.mmyz=mjso[jso].t1;
					this.myz=this.mmyz;
					
					
					
				}
			}
			
			for(var jso=0;jso<womjso.length;jso++){
				if(womjso[jso].id==contweight){
					console.log('女命：'+womjso[jso].title);
					this.ml2=this.myz+'女命称骨歌：'+womjso[jso].title;
					this.xj2=womjso[jso].content;
					this.wommyz=womjso[jso].t1;
				}
			}
			
		
			
	}}catch(e){
				//TODO handle the exception
			}
				
		
			
			
		},
		methods: {
			leftClick(){
				uni.navigateTo({
					url:'/pages/index/index'
				})
			},
			cgimg(){
				this.imgs.tfnum+=1;
				if(this.imgs.tfnum%2!=0){
					this.imgs.tfsc='../../static/ds01.png'
					this.$refs.uToast.success(`当前性别为：男`)
					this.sexico='man'
					this.xb="男"
					this.xbms="当前命理：男命"
					setTimeout(()=>{
										 uni.pageScrollTo({
										 	scrollTop:9999999,
										 	duration:0			
										 })
					},50)
					this.ml=this.ml1;
					this.xj=this.xj1;
					this.myz=this.mmyz;
				}else{
					this.imgs.tfsc='../../static/ds03.png'
					this.$refs.uToast.success(`当前性别为：女`)
					this.sexico='woman'
					this.xb="女"
					this.xbms="当前命理：女命"
					setTimeout(()=>{
										 uni.pageScrollTo({
										 	scrollTop:9999999,
										 	duration:0			
										 })
					},50)
					this.ml=this.ml2;
					this.xj=this.xj2;
					this.myz=this.wommyz;
				}
				
			},
			 click(name) {
				if (name==0) {
						 this.$refs.uToast.success(`当前性别为：男`)
						 this.sexico='man'
						 this.xb="男"
						 this.xbms="当前命理：男命"
						 this.imgs.tfsc=this.imgs.v1
						 setTimeout(()=>{
						 					 uni.pageScrollTo({
						 					 	scrollTop:9999999,
						 					 	duration:0			
						 					 })
						 },50)
						 this.ml=this.ml1;
						 this.xj=this.xj1;
						 this.myz=this.mmyz;
				} else{
					this.$refs.uToast.success(`当前性别为：女`)
					this.sexico='woman'
					this.xb="女"
					this.xbms="当前命理：女命"
					this.imgs.tfsc=this.imgs.v2
					setTimeout(()=>{
										 uni.pageScrollTo({
										 	scrollTop:9999999,
										 	duration:0			
										 })
					},50)
					this.ml=this.ml2;
					this.xj=this.xj2;
					this.myz=this.wommyz;
				}
				
			 },
			 
			 gzml(){
				 setTimeout(()=>{
					 uni.pageScrollTo({
					 	scrollTop:9999999,
					 	duration:0			
					 })
				 },50)
				 setTimeout(()=>{
				 					 uni.pageScrollTo({
				 					 	scrollTop:9999999,
				 					 	duration:0			
				 					 })
				 },200)
				 
				 setTimeout(()=>{
				 					 uni.pageScrollTo({
				 					 	scrollTop:9999999,
				 					 	duration:0			
				 					 })
				 },500)
				
			 }
			 
		}
	}
</script>

<style lang="scss">
	
 .grid-text {
        font-size: 14px;
        color: #909399;
        padding: 10rpx 0 20rpx 0rpx;
        /* #ifndef APP-PLUS */
        box-sizing: border-box;
        /* #endif */
    }
.top{
	font-weight: bolder;
}
.fg{
	text-align: center;
	font-weight: bolder;
}
.ndtp{
		margin-top: 30px;
		font-weight: bolder;
	}

.img{
	text-align: center;
	height: 230px;
}
	

.context{
	width: 100%;
	position: relative;
	bottom: 0px;
}
.tips{
	text-align: center;
	margin-top:30px ;
	font-weight: bolder;
	
}
.info{
	text-align: center;
}
</style>
