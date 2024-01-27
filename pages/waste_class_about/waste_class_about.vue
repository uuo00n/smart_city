<template>
	<view>
		<view class="">
			<view class="msg-list" style="display: flex;">
				<view style="flex: 1;">
					<image :src="host+about.imgUrl" mode="" class="pic"></image>
				</view>
				<view style="flex: 2; padding: 70rpx;">
					<view>类别：{{about.name}}</view>
					<view>分类说明：{{about.introduce}}</view>
				</view>
			</view>
		</view>
		<view>
			<view>
				<uni-section title="投放要求" sub-title="" type="circle"></uni-section>
			</view>
			<view style="padding: 50rpx;">
				{{about.guide}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				about: []
			}
		},
		mounted() {
			this.getClass()
		},
		methods: {
			getClass() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-classification/list?name=' +
						uni.getStorageSync("clsn"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.about = res.data.rows[0]
						console.log(this.about)
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>
	.pic {
		padding: 60rpx;
		width: 180rpx;
		height: 200rpx;
		border-radius: 5px;
		margin-right: 10px;
	}
</style>
