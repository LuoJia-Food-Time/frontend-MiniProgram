<template>
	<view style="padding: 20rpx;">
		<view class="box">
			<uni-forms :modelValue="form" :rules="rules" ref="formRef" label-width="160rpx" label-align="right">
				<uni-forms-item label="宿舍号" name="dormnumber" required>
					<uni-easyinput type="text" v-model="form.dormnumber" placeholder="请输入宿舍号" />
				</uni-forms-item>
				<!-- <uni-forms-item label="电话号码" name="phone" required>
					<uni-easyinput type="text" v-model="helpform.phone" placeholder="请输入电话号码" disabled />
				</uni-forms-item> -->
				<uni-forms-item label="悬赏金" name="reward" required>
					<uni-easyinput type="text" v-model="form.reward" placeholder="请输入悬赏金额" />
				</uni-forms-item>
				<!-- <uni-forms-item label="支付方式" name="payway" required>
			  	<uni-data-checkbox style="position: relative; top: 10rpx;" v-model="helpform.payway" :localdata="range"></uni-data-checkbox>
			  </uni-forms-item> -->

				<uni-forms-item>
					<button type="primary" size="mini" @click="savehelpForm">保存帮拿订单</button>
				</uni-forms-item>
			</uni-forms>
		</view>

		<view class="box" style="margin-bottom: 20rpx;">
			<view style="font-size: 36rpx; font-weight: bold; margin-bottom: 20rpx;">网络支付方式</view>
			<view style="display: flex;">
				<navigator url="/pages/pay/weChatPay" class="me-item">
					<image src="@/static/icons/微信支付.jpg" style="width:30%;" mode="widthFix"></image>
					<view>微信支付</view>
				</navigator>
				<navigator url="/pages/pay/alipay" class="me-item">
					<image src="@/static/icons/支付宝.png" style="width: 20%;" mode="widthFix"></image>
					<view>支付宝</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				form: {},
				rules: {
					dormnumber: {
						rules: [{
							required: true,
							errorMessage: '请填写宿舍号',
						}]
					},
					helptake_id: {
					  rules: [{}]
					},
					phone: {
					  rules: [{
					    required: true,
					    errorMessage: '请填写电话号码',
					  }]
					},
					reward: {
						rules: [{
							required: true,
							errorMessage: '请填写悬赏金额',
						}]
					},
					/* payway: {
				    rules: [{
				      required: true,
				      errorMessage: '请选择一种支付方式',
				    }]
				  },
				},
				range: [
					{ text: '当面支付', value: '当面支付' },
					{ text: '网络支付', value: '网络支付' },
				]
			} */
				}
			}
		},
		onLoad(option) {
			let id = option.orderId || 0
			if (id > 0) {
				this.$request.get('/orders/selectById/' + id).then(res => {
					this.form = res.data || {}
				})
			}
		},
		methods: {
			savehelpForm() {
				// 验证
				this.form.status = "已发布帮拿"

				this.$refs.formRef.validate().then(res => {
					this.$request.request({
						method: this.form.id ? 'PUT' : 'POST',
						url: this.form.id ? '/orders/update' : '/orders/add',
						data: this.form
					}).then(res => {
						if (res.code === '200') {
							uni.showToast({
								title: '操作成功'
							})

							uni.navigateBack()
						} else {
							uni.showToast({
								icon: 'error',
								title: res.msg
							})
						}
					})
				}).catch(err => {
					console.log('err', err);
				})
			}
		}
	}
</script>

<style>
	.me-item {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		grid-gap: 10rpx;
	}
</style>