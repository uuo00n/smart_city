<template>
	<view>
		<view>
			<uni-card :title="item.name" :extra="'价格:'+item.price+'元'" v-for="(item,index) in busLine">
				<view style="display: flex;">
					<view style="padding: 30rpx; flex: 1;">
						<h4>始发站：{{item.first}}</h4>
						<h4>终点站：{{item.end}}</h4>
					</view>
					<view style="padding: 30rpx; flex: 1;text-align: center;">
						<h4>首站时间：{{item.startTime}}</h4>
						<h4>末班时间：{{item.endTime}}</h4>
					</view>
				</view>
				<uni-collapse @change="getBusStation(item.id)" accordion="true">
					<uni-collapse-item title="路线图">
						<view class="content">
							<uni-steps :options="newStation" :active="0" active-color="blue"
								direction="column" />
						</view>
					</uni-collapse-item>
				</uni-collapse>
			</uni-card>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				busStationList: [],
				busLine: [],
				newStation:[]
			}
		},
		mounted() {
			this.getLine()
		},
		methods: {
			getLine() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/bus/line/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.busLine = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getBusStation(id){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/bus/stop/list?linesId='+id,
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
			}
		}
	}
</script>

<style>

</style>
