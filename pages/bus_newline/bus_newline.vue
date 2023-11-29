<template>
	<view>
		<view class="bus-map">
			<view class="">
				<uni-section title="锚点地图" sub-title="" type="line"></uni-section>
			</view>
			<view style="text-align: center;">
				<text>暂无地图数据</text>
			</view>
		</view>
		<view class="bus-line">
			<view>
				<uni-section :title="data.name+'路线图'" sub-title="" type="line"></uni-section>
			</view>
			<view class="content" style="padding: 10rpx;">
				<uni-steps :options="newStation" :active="0" active-color="blue" direction="row" />
			</view>
		</view>
		<view class="bus-msg">
			<view>
				<uni-section title="线路详细" sub-title="" type="line"></uni-section>
			</view>
			<view style="display: flex;">
				<view style="padding: 30rpx; flex: 1;">
					<h4>始发站：{{data.first}}</h4>
					<h4>终点站：{{data.end}}</h4>
				</view>
				<view style="padding: 30rpx; flex: 1;text-align: left;">
					<h4>票价：{{data.price}} 元</h4>
					<h4>里程：{{data.mileage}} KM</h4>
				</view>
			</view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="nextView">去乘车</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: [],
				busStationList: [],
				newStation: []
			}
		},
		mounted() {
			this.getBusMSg()
		},
		methods: {
			getBusMSg() {
				this.data = uni.getStorageSync("busLine")
				console.log(this.data)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/bus/stop/list?linesId=' + this.data.id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					data: {},
					success: res => {
						this.busStationList = res.data.rows
						this.newStation = this.busStationList.map(item => ({
							title: item.name
						}))
					},
					fail: () => {},
					complete: () => {}
				});
			},
			nextView(){
				uni.navigateTo({
					url: '../bus_date/bus_date',
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
