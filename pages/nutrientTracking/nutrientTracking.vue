<template>
  <view class="container">
    <view class="box">您近期的营养目标为</view>
    <radio-group @change="radioChange">
      <label v-for="(item, index) in radioList" :key="item.value" 
             :class="index === current ? 'option_active item' : 'option_default item'">
        <view class="radioHidden">
          <radio :value="item.value" :checked="index === current" />
        </view>
        <view>{{ item.value }}. {{ item.title }}</view>
      </label>
    </radio-group>
    <image class="app_content_top" src='/static/image/bgImg.jpg'></image>
    <view class="nutrition-container">
      <view class="nutrition-box">
        <view class="title">今日摄入营养</view>
        <view class="nutrition-item">热量：{{ calories }} kcal</view>
        <view class="nutrition-item">脂肪：{{ fat }} g</view>
        <view class="nutrition-item">蛋白质：{{ protein }} g</view>
      </view>
      <view class="nutrition-box">
        <view class="title">推荐摄入营养</view>
        <view class="nutrition-item">热量：{{ tarcalories[target] }} kcal</view>
        <view class="nutrition-item">脂肪：{{ tarfat[target] }} g</view>
        <view class="nutrition-item">蛋白质：{{ tarprotein[target] }} g</view>
      </view>
    </view>
	<button @click="addData">添加营养信息</button>
    <image class="app_content_middle" src='/static/image/bgImg.jpg'></image>
    <view class="diet-situation">今日您的饮食情况：{{ situation }}</view>
    
    <!-- 重叠部分 -->
    <view>{{ suggestList[suggest_num ]}} </view>
    <image class="robot-image" src="/static/image/robot.png"></image>
  </view>
</template>

<script>
  export default {
    data() {
      return {
		new_add_arr:[0,0,0],
        calories: 0,
        fat: 0,
        protein: 0,
        situation: "良好，请继续保持",
        tarcalories: [1900, 2100, 2300],
        tarfat: [20, 30, 40],
        tarprotein: [3, 4, 5],
        target: 0,
        radioList: [
          { value: '0', title: '减重' },
          { value: '1', title: '维持' },
          { value: '2', title: '增肌' }
        ],
		suggestList: [
			"主人，根据您近期摄入食物情况以及设立的营养目标分析，您本月饮食十分健康，请继续保持吧！",
			"主人，根据您近期摄入食物情况以及设立的营养目标分析，您本月摄入高热量食物太多，请适当减少此类食物摄入吧！",
			"主人，根据您近期摄入食物情况以及设立的营养目标分析，您本月摄入，请适当摄入更多的高蛋白食物吧！"
		],
		suggest_num : 1 ,
        current: 0
      }
    },
    onLoad() {},
	onShow(){
		let that = this;
		uni.$on('id',function(ID){
			that.new_add_arr = ID;
		});
		that.add();
	},
    methods: {
      addData(){
		  uni.navigateTo({
		  	url:'/pages/blogPopup/blogPopup'
		  })
	  },
	  add(){
		  this.calories += this.new_add_arr[0];
		  this.fat += this.new_add_arr[1];
		  this.protein += this.new_add_arr[2];
	  },
      radioChange(evt) {
        for (let i = 0; i < this.radioList.length; i++) {
          if (this.radioList[i].value === evt.detail.value) {
            this.current = i;
            break;
          }
        }
        this.target = this.radioList[this.current].value;
        console.log(this.target);
      }
    }
  }
</script>

<style scoped>
.container {
  padding: 20rpx;
  background-color: rgb(240, 234, 210);
}

.box {
  margin-bottom: 20rpx;
  font-size: 28rpx;
}

.radio-group {
  margin-bottom: 20rpx;
}

.item {
  display: flex;
  align-items: center;
  padding: 16rpx 27rpx;
  margin-bottom: 10rpx;
  border-radius: 31rpx;
  font-size: 30rpx;
  color: #FFFFFF;
}

.option_active {
  background: #ff5500;
}

.option_default {
  background: #ffaa7f;
}

.radioHidden {
  display: none;
}

.app_content_top, .app_content_middle {
  width: 100%;
  height: auto;
  background-size: cover;
  opacity: 0.1;
}

.nutrition-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20rpx;
}

.nutrition-box {
  flex: 1;
  margin: 0 10rpx;
}

.title {
  font-size: 28rpx;
  font-weight: bold;
  margin-bottom: 10rpx;
}

.nutrition-item {
  font-size: 28rpx;
  margin-bottom: 10rpx;
}

.diet-situation {
  margin-top: 20rpx;
  font-size: 28rpx;
}

/* 重叠部分 */
.button-container {
  position: relative;
  width: 45%;
  margin: 20rpx auto;
}

.button-image {
  width: 100%;
}

.button-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: black;
  font-size: 28rpx;
  font-weight: bold;
  text-align: center;
  cursor: pointer;
}
.robot-image {
  display: block;
  width: 142rpx;
  height: 195rpx;
  margin: 20rpx auto;
}
</style>
