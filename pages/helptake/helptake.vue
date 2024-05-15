<template>
	<view>
		<uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" styleType="text"
			activeColor="#ff9900"></uni-segmented-control>
			<view style="padding: 20rpx;">
				<view class="box" v-for="item in ordersList" :key="item.id" style="margin-bottom: 10rpx;">
					<view style="display: flex; align-items: baseline; margin-bottom: 10rpx;">
						<navigator :url="'/pages/detail/detail?businessId=' + item.businessId"
							style="flex: 1; font-size: 32rpx; ">{{ item.businessName }}
							<uni-icons type="right" size="16" color="#666"
								style="position: relative; top: 2rpx;"></uni-icons>
						</navigator>
						<view style="font-size: 24rpx; color: #666;">{{ item.status }}</view>
					</view>
					<view style="display: flex; align-items: center; grid-gap: 20rpx; margin-bottom: 10rpx;"  @click="goOrdersItem(item.id)">
						<view>
							<image style="display: block; width: 160rpx; height: 160rpx; border-radius: 10rpx;"
								:src="item.cover"></image>
						</view>
						<view style="flex: 1;">{{ item.name }}</view>
						<view>实付￥<text style="font-size: 36rpx; font-weight: bold; color: red;">{{ item.actual }}</text>
						</view>
					</view>
					<view style="display: flex; min-height: 40rpx;">
						<view style="flex: 1; text-align: right;">
							<uni-tag v-if="item.status === '已发布帮拿'" text="接受帮拿" size="mini" type="primary"
								@click="changeStatus(item, '已接受帮拿', user.id)"></uni-tag>
						</view>
					</view>
				</view>
			
			</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current: 0,
				items: ['帮拿查询', '已发布', '已接受'],
				ordersList: [],
				user: uni.getStorageSync('xm-user')
			}
		},
		onShow() {
			this.loadOrders()
		},
		methods: {
			goOrdersItem(orderId) {
				uni.navigateTo({
					url: '/pages/ordersItem/ordersItem?orderId=' + orderId
				})
			},
			changeStatus(orders, status, id) {
				let form = JSON.parse(JSON.stringify(orders))
				form.status = status
				form.helptake_id = id
				this.$request.put('/orders/update', form).then(res => {
					if (res.code === '200') {
						uni.showToast({
							icon: "success",
							title: '操作成功'
						})
						this.loadOrders()
					} else {
						uni.showToast({
							icon: "error",
							title: res.msg
						})
					}
				})
			},
			loadOrders() {
				let status = '帮拿查询'
				switch (this.current) {
					case 0:
						status = '帮拿查询';
						this.$request.get('/orders/selectAll', {
							status: '已发布帮拿'
						}).then(res => {
							this.ordersList = res.data || []
						})
						break
					case 1:
						status = '已发布';
						this.$request.get('/orders/selectAll', {
							userId: this.user.id,
							status: '已发布帮拿'
						}).then(res => {
							this.ordersList = res.data || []
						})
						break
					case 2:
						status = '已接受';
						this.$request.get('/orders/selectAll', {
							helptake_id: this.user.id,
							status: '已接受帮拿'
						}).then(res => {
							this.ordersList = res.data || []
						})
						break
				}
			},
			onClickItem(e) {
				if (this.current != e.currentIndex) {
					this.current = e.currentIndex;
					this.loadOrders()
				}
			}
		}
	}
</script>

<style>

</style>
