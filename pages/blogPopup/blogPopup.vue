<template>
	<view class="informantion_mask" @tap="closeWindow">
		<view class="informantion_content" @tap.stop.prevent>
			<view class="mask-header">
				<image src="../../static/image/300.jpg" mode="aspectFit" @click="closeWindow"></image>
			</view>
			<view class="informantion-title">
				<p class="informantion-title-p">添加营养信息</p>
				<input v-model.number="inputCalories" type="number" placeholder="请输入热量 /kal" />  
				<input v-model.number="inputFat" type="number" placeholder="请输入脂肪 /g" />  
				<input v-model.number="inputProtein" type="number" placeholder="请输入蛋白质 /g" />
				<view>{{addC}}</view>
			</view>
			<view class="informantion-btn">
				<button type="primary" @click="returnWindow">确认</button>
			</view>
		</view>
	</view>
</template>
 
<script>
	export default {
		data() {  
		    return {  
		      // 使用Number类型来存储，但注意v-model.number仍然可能返回Number类型的非整数  
		      inputCalories: NaN,  
		      inputFat: NaN,  
		      inputProtein: NaN,  
		      // 整数数组，用于存储转换后的值  
		      add_arr: [0, 0, 0]  
		    };  
		  },  
		  watch: {  
		    // 监听inputCalories的变化，并在变化时更新add_arr[0]  
		    inputCalories(newVal) {  
		      this.add_arr[0] = Math.floor(newVal) || 0; // 使用Math.floor确保是整数，并处理NaN情况  
		    },  
		    inputFat(newVal) {  
		      this.add_arr[1] = Math.floor(newVal) || 0;  
		    },  
		    inputProtein(newVal) {  
		      this.add_arr[2] = Math.floor(newVal) || 0;  
		    }  
		  },
		methods: {
			updateC(e){
				this.addC = e.target.value;
				this.add_arr[0] = this.addC;
			},
			updateF(e){
				this.addF = e.target.value;
				this.add_arr[1] = parseInt(this.addF,10);
			},
			updateP(e){
				this.addP = e.target.value;
				this.add_arr[2] = parseInt(this.addP,10);
			}
			,closeWindow() {
				uni.navigateBack({
					delta: 1,
					})
			},
			returnWindow(){
				uni.$emit('id',this.add_arr)
				uni.navigateBack({
					delta: 1,
					})
			}

		}
	}
</script>
 
<style lang="scss">
	page {
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.4);
	}
 
	.informantion_mask {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}
 
	.informantion_content {
		width: 600rpx;
		height: 820rpx;
		overflow: hidden;
		border-radius: 10rpx;
		background-color: white;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		padding-bottom: 20rpx;
	}
 
	.mask-header {
		height: 400rpx;
		position: relative;
		background-image: url("../../static/image/200.webp");
		background-repeat: no-repeat;
		background-size: cover;
		image {
			width: 40rpx;
			height: 40rpx;
			position: absolute;
			top: 20rpx;
			right: 20rpx;
		}
	}
	
	.informantion-title {
		text-align: center;
		padding: 0 40rpx;
		p {
			font-size: 35rpx;
			font-weight: bold;
			line-height: 80rpx;
		}
		span {
			color: #6C6C6C;
		}
	}
 
	.informantion-btn {
		height: 93rpx;
		width: 80%;
		margin: 0 auto;
	}
</style>
