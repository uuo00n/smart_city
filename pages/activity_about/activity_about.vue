<template>
	<view>
		<view class="data">
			<view class="pic">
				<image :src="host+data.imgUrl" mode="" style="width: 100%;"></image>
			</view>
			<view class="title">
				<uni-section :title="data.name" :sub-title="'活动类别:'+data.categoryName" type="circle"></uni-section>
			</view>
			<view class="body" style="padding: 20rpx;">
				{{data.content}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				data:[]
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/activity/'+uni.getStorageSync("act_id"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.data = res.data.data
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
