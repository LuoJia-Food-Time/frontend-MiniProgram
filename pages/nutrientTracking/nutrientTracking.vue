<template>
	<view>
	  <view class="box" style="">您近期的营养目标为</view>
	  <radio-group @change="radioChange">
	  	<label v-for="(item, index) in radioList" :key="item.value"
	  		:class="index === current?'option_active item':'option_default item'">
	  		<view class="radioHidden">
	  			<radio :value="item.value" :checked="index === current" />
	  		</view>
	  		<view>{{item.value}}. {{item.title}}</view>
	  	</label>
	  </radio-group>
	  <image class="app_content_top" src='/static/image/bgImg.jpg'></image>
	  <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:12%;top: 23%;"> 今日摄入营养 </view>
	  <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:11%;top: 29%;"> 热 &nbsp;&nbsp;&nbsp;量：{{calories}} kcal </view>
	  <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:11%;top: 33%;"> 脂 &nbsp;&nbsp;&nbsp;肪：{{fat}} g </view>
	 <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:11%;top: 37%;"> 蛋白质：{{protein}} g</view>
	 <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:51%;top: 23%;"> 推荐摄入营养 </view>
	 <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:50%;top: 29%;"> 热 &nbsp;&nbsp;&nbsp;量：{{tarcalories[target]}} kcal </view>
	 <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:50%;top: 33%;"> 脂 &nbsp;&nbsp;&nbsp;肪：{{tarfat[target]}} g </view>
	 <view class="box" style="font-size: 28rpx; font-weight: 60rpx;position: absolute;left:50%;top: 37%;"> 蛋白质：{{tarprotein[target]}} g</view>
	 
	 
	<image class="app_content_top" style="top: 50%;height: 40%;"   src='/static/image/bgImg.jpg'></image>
	<view class="box" style="top:53%;left: 22%;position: absolute;font-size: 28rpx;">本月您的饮食情况{{situation}}</view>
	<image class="app_content_top" style="top:56%;left:27%;opacity: 1;height: 15%;width: 45%;" src='/static/image/button.png' ></image>
    <view  style="color: black;font-size: 28rpx; font-weight: 60rpx;position: absolute;left:36%;top: 62%;" @click="goToData">
    	查看本月营养数据
    </view>
	<image class="app_content_top" style="top: 72%;left: 5%;opacity: 1;height: 195rpx;width: 142rpx" src="/static/image/robot.png"></image></view>
</template>

<script>
	export default {
		data() {
			return {
				calories:100,
				fat:2222,
				protein:300,
				situation:"良好，请继续保持",
			    tarcalories:[
					200,
					300,
					400
				],
				tarfat:[
					20,
					30,
					40
				],
				tarprotein:[
					3,
					4,
					5
				],
				target:0,
				radioList: [{
								value: '0',
								title: '减重',
							},
							{
								value: '1',
								title: '维持'
							},
							{
								value: '2',
								title: '增肌'
							}
						],
				current:0
			}
		},
		onLoad() {

		},
		
		methods: {
			goToData(){
				uni.navigateTo({
					url:'/pages/nutrientData/nutrientData'
				})
			},
			
			radioChange(evt) {
					for (let i = 0; i < this.radioList.length; i++) {
						if (this.radioList[i].value === evt.detail.value) {
							this.current = i;
							break;
						}
					}
					this.target = this.radioList[this.current].value
					console.log(this.target);
				},
		}
	}
</script>

<style>
	page{
		background-color: rgb(240, 234, 210);
	}
	.app_content_top {
			z-index: -1;
			background-size: contain;
			top:20%;
			height: 27.5%;
			width: 100%;
			left: 0%;
			position: absolute;
			flex-direction: column;
			align-items: left;
			opacity: 0.1;
		}
	.item {
		display: flex;
		padding: 16rpx 27rpx;
		margin-bottom: 10rpx;
		background: #ffaa7f;
		border-radius: 31rpx;
		font-size: 30rpx;
		color: #FFFFFF;
	}
	
	// 选中的颜色
	.option_active {
		background: #ff5500;
	}
	
	// 默认颜色
	.option_default {
		background: #ffaa7f;
	}
	.radioHidden {
		display: none;
	}
</style>



