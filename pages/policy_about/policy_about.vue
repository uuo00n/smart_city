<template>
	<view>
		<uni-title type="h2" :title="policyData.title" align="center"></uni-title>
		<view style="text-align: center;">
			<view>{{policyData.author}}</view>
			<view>{{policyData.createTime}}</view>
		</view>
		<view class="font" style="padding:10rpx 50rpx;">
			{{policyData.content}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				policyData:[]
			}
		},
		mounted() {
			this.getPolicyMsg()
		},
		methods: {
			getPolicyMsg(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/talent-policy/'+uni.getStorageSync("policyMsg").id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.policyData = res.data.data
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
