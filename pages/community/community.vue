<template>
 <view class="container">
    <!-- 搜索框 -->
    <view class="box">
      <uni-section title="基本用法" type="line">
        <uni-search-bar @confirm="search" :focus="true" v-model="searchValue"></uni-search-bar>
      </uni-section>
    </view>

    <!-- 卡片列表 -->
    <view class="card-list">
      <uni-section v-for="(card, index) in cards" :key="index" class="uni-card">
        <uni-card :cover="card.cover" @click="onClick(card)" class="">
          <text>{{ card.content |  truncate(30) }}</text>
          <view slot="actions" class="card-actions">
            <view class="card-actions-item" @click="actionsClick('点赞')">
              <uni-icons type="heart" size="18" color="#999"></uni-icons>
              <text class="card-actions-item-text">点赞</text>
            </view>
          </view>
        </uni-card>
      </uni-section>
    </view>

    <!-- 分页器 -->
    <view class="box">
      <uni-section title="默认样式" type="line" padding>
        <uni-pagination :current="current" :total="100" title="标题文字" :show-icon="true" @change="change" />
        <!-- 当前页：{{ current }} -->
      </uni-section>
    </view>

    <!-- 悬浮按钮 -->
    <uni-fab ref="fab" :pattern="pattern" :content="content" :horizontal="horizontal" :vertical="vertical"
      :direction="direction" @fabClick="fabClick" @trigger="trigger" />
  </view>
</template>

<script>
export default {
	data() {
		return {
			searchValue: '',
			extraIcon: {
				color: '#4cd964',
				size: '22',
				type: 'gear-filled'
			},
			cards:[
				  {
					"title": "奈雪的茶",
					"cover": "http://localhost:9090/files/1698200628077-微信截图_20231019092733.png",
					"content": "\n🍵 奈雪的茶 🍵\n奈雪的茶，非常奈斯！新鲜的奶茶搭配精致的茶点，让你感受茶与奶的完美结合。\n🔥 推荐口味：水果茶、经典奶茶、健康轻饮\n⏰ 营业时间：9:00 - 21:00\n📍 地址：街道口\n☎️ 电话：0551-67889988\n快来享受美味吧！\n"
				  },
				  {
					"title": "汉堡王",
					"cover": "http://localhost:9090/files/1698201477026-QQ截图20231022180706.png",
					"content": "\n🍔 汉堡王 🍔\n量大好吃的汉堡王来啦！我们提供各种美味汉堡，让你大快朵颐。\n🔥 推荐口味：招牌汉堡、芝士汉堡、鸡肉汉堡\n⏰ 营业时间：9:00 - 22:00\n📍 地址：广埠屯\n☎️ 电话：13788997788\n快来品尝吧！\n"
				  },
				  {
					"title": "奶茶王",
					"cover": "http://localhost:9090/files/1698288093924-微信截图_20231019092635.png",
					"content": "\n🧋 奶茶王 🧋\n美味奶茶，非常棒棒！精选上等茶叶和新鲜牛奶，带给你丝滑的口感。\n🔥 推荐口味：原味奶茶、珍珠奶茶、红豆奶茶\n⏰ 营业时间：9:00 - 21:00\n📍 地址：武大路\n☎️ 电话：13788997788\n快来品尝吧！\n"
				  },
				  {
					"title": "幸运咖",
					"cover": "http://localhost:9090/files/1698288093924-微信截图_20231019092635.png",
					"content": "\n☕ 幸运咖 ☕\n非常棒棒的咖啡店！来一杯香浓的咖啡，享受悠闲的时光。\n🔥 推荐口味：拿铁、卡布奇诺、美式咖啡\n⏰ 营业时间：9:00 - 21:00\n📍 地址：武大路\n☎️ 电话：13788997788\n快来享受美味吧！\n"
				  },
				  {
					"title": "小武哥哥",
					"cover": "http://localhost:9090/files/1698998161478-咖啡.png",
					"content": "\n🧋 小武哥哥 🧋\n小武哥哥是真的骚！我们提供各种特色奶茶，让你一饮倾心。\n🔥 推荐口味：原味奶茶、抹茶奶茶、芒果奶茶\n⏰ 营业时间：9:00 - 21:00\n📍 地址：珞珈山\n☎️ 电话：13899778899\n快来享受美味吧！\n"
				  }
				],
			  // cards: [
			  //   {
				 // title: "意大利披萨",
			  //     cover: 'path/to/image1.jpg',
			  //     content: ```
				 //  🍕 美味意大利披萨 🍕
				 //  想要尝试正宗的意大利披萨吗？我们的披萨采用最新鲜的食材，手工制作的面团，烘焙至金黄酥脆，搭配丰富的芝士和新鲜的配料，带给您难忘的美食体验！
				 //  🔥 新品推荐：玛格丽特披萨、四季披萨、夏威夷披萨
				 //  ⏰ 营业时间：上午10:00 - 晚上10:00
				 //  📍 地址：市中心美食广场2楼
				 //  ☎️ 电话：123-456-7890
				 //  快来享受美味吧！
				 //  ```
				  
			  //   },
			  //   {
			  //   	title: "日式拉面",
			  //     cover: 'path/to/image1.jpg',
			  //     content: 
				 //  ```
				 //  🍜 正宗日式拉面 🍜
				 //  寒冷的冬天，来一碗热气腾腾的日式拉面，温暖你的胃！我们的拉面汤底经过长时间熬制，浓郁香滑，搭配劲道的拉面和丰富的配料，满足你的味蕾！
				 //  🔥 推荐口味：豚骨拉面、味噌拉面、海鲜拉面
				 //  ⏰ 营业时间：上午11:00 - 晚上9:00
				 //  📍 地址：市中心美食街8号
				 //  ☎️ 电话：098-765-4321
				 //  快来品尝吧！
				 //  ```
			  //   }, {
					// title: "法式甜点",
			  //     cover: 'path/to/image1.jpg',
			  //     content: 
				 //  ```
				 //  🍰 精致法式甜点 🍰
				 //  来品尝我们精心制作的法式甜点，每一口都带给你甜蜜的享受！选用优质原料，精湛的烘焙技术，带给你无与伦比的美味体验。
				 //  🔥 推荐甜点：马卡龙、歌剧蛋糕、香草泡芙
				 //  ⏰ 营业时间：上午9:00 - 下午6:00
				 //  📍 地址：市中心步行街5号
				 //  ☎️ 电话：321-654-0987
				 //  欢迎光临！
				 //  ```
			  //   }, {
					// title: "地中海沙拉",
			  //     cover: 'path/to/image1.jpg',
			  //     content: 
				 //  ```
				 //  🥗 健康地中海沙拉 🥗
				 //  为你的健康加分，来试试我们的地中海沙拉吧！新鲜的蔬菜搭配优质的橄榄油和香料，既美味又健康，是你日常饮食的绝佳选择。
				 //  🔥 推荐沙拉：希腊沙拉、鹰嘴豆沙拉、烤蔬菜沙拉
				 //  ⏰ 营业时间：上午10:00 - 下午8:00
				 //  📍 地址：市中心健康美食区3号
				 //  ☎️ 电话：567-890-1234
				 //  健康生活，从这里开始！
				 //  ```
			  //   }
			  // ],
			  
			  
			  content: [
			  					
			  					{
			  						iconPath: '/static/star.png',
			  						selectedIconPath: '/static/star-active.png',
			  						text: '发布帖子',
			  						active: false
			  					}
			  				],
							
		current: 1
		};
	},
	methods: {
		search(res) {
			uni.showToast({
				title: '搜索：' + res.value,
				icon: 'none'
			});
		},
		onClick(e) {
			 const detail =  encodeURIComponent(JSON.stringify(e));
			
			uni.navigateTo({
								url:'/pages/post/post?detail='+detail
							});
		},
		actionsClick(text) {
			uni.showToast({
				title: text,
				icon: 'none'
			});
		},
		onBackPress() {
			// #ifdef APP-PLUS
			plus.key.hideSoftKeybord();
			// #endif
		},
		fabClick() {
						
					},
		change(e) {
						console.log(e)
						this.current = e.current
					},
		trigger(e) {
						console.log(e)
						this.content[e.index].active = !e.item.active
						uni.navigateTo({
							url: '/pages/addPost/addPost'
						});
					},
	}
};

</script>

<style>
.container {
  padding: 20rpx;
}

.box {
  margin-bottom: 20rpx;
}

.card-list {
  margin-bottom: 20rpx;
}

.uni-card {
  margin-bottom: 20rpx;
}

.card-actions {
  display: flex;
  align-items: center;
}

.card-actions-item {
  margin-right: 10rpx;
  display: flex;
  align-items: center;
}

.card-actions-item-text {
  margin-left: 5rpx;
}
	
	
</style>
