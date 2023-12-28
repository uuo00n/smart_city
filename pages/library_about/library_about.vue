<template>
	<view>
		<view class="pic">
			<image :src="host+libData.imgUrl" mode="" style="width: 100%;"></image>
		</view>
		<uni-title :title="libData.name" type="h1" style="padding: 10rpx;"></uni-title>
		<view class="body" style="padding: 30rpx;">
			<view>位置：{{libData.address}}</view>
			<view style="text-align: center;"><image :src="host+this.mapUrlNew" mode="aspectFit"></image></view>
			<view>营业时间：{{libData.businessHours}}</view>
			<view>营业状态：<text v-if="libData.businessState ==1 " style="color: #18BC37;">营业中</text><text v-else
					style="color: #E43D33;">休息中</text></view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="goComment">查看评论</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				libData: [],
				mapUrlNew:""
			}
		},
		mounted() {
			this.getLibData()
		},
		methods: {
			getLibData() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/digital-library/library/' + uni.getStorageSync(
						"lib_id"),
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.libData = res.data.data
						const map = res.data.data.mapUrl;
						this.mapUrlNew = map.replace("/dev-api", "/prod-api");
						console.log(this.mapUrlNew)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goComment(){
				uni.navigateTo({
					url: '../library_comment/library_comment',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
