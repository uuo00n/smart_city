<template>
	<view>
		<view class="title pad">
			<uni-title :title="actData.title" type="h1"></uni-title>
		</view>
		<view class="body" style="padding: 20rpx;">
			<view>活动简介：{{actData.detail}}</view>
			<view>活动开始时间：{{actData.startAt}}</view>
			<view>人员要求：{{actData.requireText}}</view>
			<view>承办单位：{{actData.undertaker}}</view>
			<!-- <view>工作内容：{{}}</view> -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				actData: []
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData() {
				console.log(uni.getStorageSync("vol_act_id"))
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/activity/' + uni
						.getStorageSync("vol_act_id"),
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.actData = res.data.data
						console.log(this.actData)
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.pad {
		padding: 10rpx;
	}
</style>
