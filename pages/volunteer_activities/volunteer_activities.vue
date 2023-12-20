<template>
	<view>
		<view class="search-atv">
			<view>
				<uni-section title="活动搜索" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<view class="search-box">
					<uni-search-bar cancelText="搜索" @cancel="goSearch"></uni-search-bar>
				</view>
			</view>
		</view>
		<view class="list">
			<view>
				<uni-section title="活动列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item v-for="(item,index) in listData" :key="index" clickable>
						<view slot="body" @click="goAbout(item)">
							<view>
								<view>{{item.title}}</view>
								<view style="font-size: 25rpx;">
									<view>承办单位：{{item.undertaker}}</view>
									<view>开始时间：{{item.startAt}}</view>
									<view>人员要求：{{item.requireText}}</view>
								</view>
							</view>
						</view>
						<view slot="footer" class="fixed-button">
							<button type="primary" size="mini" @click="application(item)">报名</button>
						</view>
					</uni-list-item>
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
				listData: [],
				searObj:[]
			}
		},
		mounted() {
			this.getList()
		},
		methods: {
			// 搜索功能
			// goSearch(e) {
			// 	console.log(e)
			// 	this.searObj = this.listData.filter(item => item.title.includes(e));
			// 	console.log(this.searObj)
			// },
			getList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/activity/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.listData = res.data.rows
						console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(e) {
				uni.setStorageSync("vol_act_id", e.id)
				uni.navigateTo({
					url: '../volunteer_act_about/volunteer_act_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			application(e) {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/register',
					method: 'POST',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {
						"activityId": e.id,
						"newState": true
					},
					success: res => {
						if(res.data.code === 200){
							uni.showToast({
								title: '报名成功',
								position: 'center',
								duration: 3000
							});
						}else{
							uni.showToast({
								title: '报名失败：'+res.data.msg,
								position:'bottom',
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
	.fixed-button {
		position: absolute;
		right: 0;
		bottom: 0;
		margin-right: 20rpx;
	}
</style>
