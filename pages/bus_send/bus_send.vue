<template>
	<view>
		<view>
			<uni-section title="订单详细" sub-title="" type="circle"></uni-section>
		</view>
		<view style="padding: 50rpx;" class="msg-list">
			<view>乘车人：{{name}}</view>
			<view>联系电话：{{phoneNumber}}</view>
			<view>乘车线路：{{line.name}}</view>
			<view>上车点：{{start}}</view>
			<view>下车点：{{end}}</view>
			<view>乘车日期：{{date}}</view>
		</view>
		<view class="bottom_text">
			<view style="margin: 10rpx;">
				<button type="primary" size="default" @click="sendQs">提交订单</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				line: [],
				name: '',
				phoneNumber: '',
				start: '',
				end: '',
				date: ''
			}
		},
		mounted() {
			this.getSend()
		},
		methods: {
			getSend() {
				this.line = uni.getStorageSync("busLine")
				this.name = uni.getStorageSync("userName")
				this.phoneNumber = uni.getStorageSync("userPhone")
				this.start = uni.getStorageSync("start")
				this.end = uni.getStorageSync("end")
				this.date = uni.getStorageSync("busLineDate")
			},
			sendQs() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/bus/order',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					method: 'POST',
					data: {
						"start": this.start,
						"end": this.end,
						"price": "8",
						"path": this.line.name,
						"status": 9
					},
					success: res => {
						if (res.data.code === 200) {
							uni.showToast({
								title: '预约成功',
								position: 'bottom',
								icon: 'none',
								success() {
									setTimeout(() => {
										uni.navigateBack({
											delta: 4
										});
									}, 3000);
								}
							});
						} else {
							uni.showToast({
								title: '提交失败，失败原因：' + data.msg,
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.msg-list view {
		margin: 10rpx;
	}

	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
