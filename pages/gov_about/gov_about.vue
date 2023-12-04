<template>
	<view>
		<view>
			<view>
				<image :src="host+aboutData.imgUrl" mode="" style="width: 100%;"></image>
			</view>
			<uni-title :title="aboutData.title" align="center" type="h1"></uni-title>
			<view style="text-align: center; margin: 10rpx;">
				<view>{{aboutData.undertaker}}</view>
				<view>{{aboutData.createTime}}</view>
			</view>
			<view style="padding: 40rpx;">
				{{aboutData.content}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				aboutData: []
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal/' + uni
						.getStorageSync("gov_about").id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.aboutData = res.data.data
						console.log(this.aboutData)
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
