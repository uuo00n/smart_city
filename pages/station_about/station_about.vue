<template>
	<view>
		<view class="pic">
			<image :src="host+stationMsg.imgUrl" mode="" style="width: 100%;"></image>
		</view>
		<view class="body">
			<uni-title :title="stationMsg.name" type="h1" style="padding: 0rpx 20rpx;"></uni-title>
			<view style="padding: 30rpx;">
				<view>简历：{{stationMsg.introduce}}</view>
			</view>
		</view>
		<view class="policy">
			<view>
				<uni-section title="政策列表" sub-title="" type="circle"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.title" :note="item.author" v-for="(item,index) in PolicyList" @click="goPolicyAbo(item)" clickable></uni-list-item>
				</uni-list>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				stationMsg:[],
				PolicyList:[]
			}
		},
		mounted() {
			this.getMsg()
			this.getPolicy()
		},
		methods: {
			getMsg(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/talent-policy-area/'+uni.getStorageSync("stationMsg").id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.stationMsg = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getPolicy(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/talent-policy/list?areaId='+uni.getStorageSync("stationMsg").id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.PolicyList = res.data.data
						console.log(this.PolicyList)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goPolicyAbo(e){
				uni.setStorageSync("policyMsg",e)
				uni.navigateTo({
					url: '../policy_about/policy_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
