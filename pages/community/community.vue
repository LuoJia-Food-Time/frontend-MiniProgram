<template>
	<view style="padding: 20rpx">
		<view class="box">
			<uni-section title="基本用法" type="line">
				<uni-search-bar @confirm="search" :focus="true" v-model="searchValue"></uni-search-bar>
			</uni-section>
		</view>
		<view class="card-list0">
		<uni-section v-for="(card, index) in cards" :key="index" class="uni-card0">
		  <uni-card :cover="card.cover" @click="onClick(card)" class=''>
		    <text >{{ card.content }}</text>
		          <view slot="actions" class="card-actions">
		            <view class="card-actions-item" @click="actionsClick('点赞')">
		              <uni-icons type="heart" size="18" color="#999"></uni-icons>
		              <text class="card-actions-item-text">点赞</text>
		            </view>
		          </view>
		        </uni-card>
		      </uni-section>
		</view>
		
		<uni-section title="默认样式" type="line" padding>
					<uni-pagination :current="current" :total="100" title="标题文字" :show-icon="true" @change="change" />
					当前页：{{ current }}
			</uni-section>
		
		<uni-fab ref="fab" :pattern="pattern" :content="content" :horizontal="horizontal" :vertical="vertical"
					:direction="direction"  @fabClick="fabClick" @trigger="trigger"/>
					
					
			</view>
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
			  cards: [
			    {
			      cover: 'path/to/image1.jpg',
			      content: '第一张卡片的内容'
			    },
			    {
			      cover: 'path/to/image2.jpg',
			      content: '第二张卡片的内容'
			    },
				{
				  cover: 'path/to/image1.jpg',
				  content: '第一张卡片的内容'
				},
				{
				  cover: 'path/to/image2.jpg',
				  content: '第二张卡片的内容'
				},
				{
				  cover: 'path/to/image1.jpg',
				  content: '第一张卡片的内容'
				},
				{
				  cover: 'path/to/image2.jpg',
				  content: '第二张卡片的内容'
				}
			  ],
			  
			  
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
			uni.navigateTo({
								url:'/pages/post/post?detail=+JSON.stringify(this.item)'
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
						uni.showToast({
							title: '点击了悬浮按钮',
							icon: 'none'
						})
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
.search-result {
	padding-top: 10px;
	padding-bottom: 20px;
	text-align: center;
}

.search-result-text {
	text-align: center;
	font-size: 14px;
	color: #666;
}

.example-body {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	padding: 0px;
}

.uni-mt-10 {
	margin-top: 10px;
}
.card-list0 {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around; 
}

.uni-card0 {
 width: calc(50% - 5px);
  margin-bottom: 20px;
}

.warp {
		padding: 10px;
	}

	.button {
		margin-bottom: 10px;
	}
	
	
	
	
</style>
