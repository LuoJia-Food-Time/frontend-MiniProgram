<template>
	<view>
		<view style="display: flex; justify-content: center; align-items: center;"> 
		
		<text class="title">{{info.title}}</text>
		</view>
		<view class="">
			<text class="content">   {{'   '}}   {{ info.content}}</text>
		</view>
		<view><image :src="info.imageUrl" style="width: 100%;">
		
		</image></view>
		
	</view>
</template>

<script>

	export default {
		
		data() {
			return {
				// 当前帖子信息
				info: {
					title: "帖子标题",
					content: "内容",
					imageUrl: "../../static/imgs/汉堡.png"
				}
			}
		},
		
		// 接受传参
		onLoad(e) {
			// 初始化
			if (e.detail){
				info = JSON.parse(e.detail)
				this.__init(JSON.parse(e.detail))
			}
			
		},
		methods: {
			__init(data){
				// 修改标题
				console.log(data)
				if(data){
					uni.setNavigationBarTitle({
						title:data.title
					})
				}
				else{
					uni.setNavigationBarTitle({
						title: "帖子"
					})
				}
			}
			
		}
	}
</script>

<style>
.title{
	font-size: 40px;
}
.content{
	font-size: 25px;
}
</style>

