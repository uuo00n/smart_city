<template>
	<view>
		<view>
			<uni-title :title="data.title" align="center" type="h1"></uni-title>
			<view style="text-align: center;">
				<view>{{data.author}}</view>
				<view>{{data.createTime}}</view>
			</view>
		</view>
		<view>
			<view style="text-align: center;">
				<image :src="host+data.imgUrl" mode="aspectFit"></image>
			</view>
			<view class="font" style="padding: 10rpx;">
				{{data.content}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				data:[]
			}
		},
		mounted() {
			this.getNewAbo()
		},
		methods: {
			getNewAbo(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news/'+uni.getStorageSync("waste_news").id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.data = res.data.data
						console.log(this.data)
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
