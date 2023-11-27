<template>
	<view>
		<h1 style="margin: 30rpx;">{{allStation.name}}</h1>
		<view class="header">
			<view style="display: flex;">
				<view style="padding: 30rpx; flex: 1;">
					<h4>始发站：{{allStation.first}}</h4>
					<h4>终点站：{{allStation.end}}</h4>
				</view>
				<view style="padding: 30rpx; flex: 1;text-align: center;">
					<h4>首站时间：{{allStation.startTime}}</h4>
					<h4>末班时间：{{allStation.endTime}}</h4>
				</view>
			</view>
			<view class="Station">
				<uni-section :title="'当前位置：'+thisStation.currentName" :sub-title="'列车运行到站：'+allStation.runStationsName"
					type="circle">
				</uni-section>
			</view>
			<view style="padding: 30rpx;">
				<h4>剩余时间：{{allStation.remainingTime}}分钟</h4>
				<h4>间隔：{{allStation.stationsNumber}}站</h4>
			</view>
		</view>
		<view style="margin:10rpx 50rpx;">
			<uni-steps :options="newStation" :active="find(allStation.runStationsName)" active-color="blue"
				direction="column" />
			</uni-card>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				thisStation: [],
				allStation: [],
				newStation: []
			}
		},
		mounted() {
			this.getMetro()
		},
		methods: {
			getMetro() {
				this.thisStation = uni.getStorageSync("met_msg")
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/metro/line/' + this.thisStation.lineId,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.allStation = res.data.data
						this.newStation = this.allStation.metroStepList.map(item => ({
							title: item.name
						}))
					},
					fail: () => {},
					complete: () => {}
				});
			},
			find(e) {
				return this.newStation.findIndex(item => item.title === e)
			}
		},
		onNavigationBarButtonTap(e) {
			uni.navigateTo({
				url: '../metro_all/metro_all',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		},
	}
</script>

<style>

</style>
