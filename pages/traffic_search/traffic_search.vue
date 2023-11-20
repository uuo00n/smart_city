<template>
	<view>
		<view>
			<view>
				<uni-section title="搜索" sub-title="" type="line"></uni-section>
			</view>
			<view class="search">
				<uni-search-bar @clear="search" cancelButton="none" clearButton="none" placeholder="省份" style="flex: 1;"
					v-model="city"></uni-search-bar>
				<uni-search-bar @confirm="search" cancelText="确认" @cancel="search" placeholder="车牌号" style="flex: 2;"
					v-model="carNo"></uni-search-bar>
			</view>
		</view>
		<view v-if="this.count != 0">
			<view>
				<uni-section title="搜索结果" sub-title="" type="line"></uni-section>
			</view>
			<view v-if="this.dataList.length != 0">
				<view>
					<uni-list>
						<uni-list-item v-for="(item,index) in dataList.slice(0,this.visibleCount)" :key="index"
						:clickable="true" @click="tapListItem(item)">
							<view slot="header" style="width: 250px;">
								<h6>违法时间：</h6>
								<h6>违章地点：</h6>
								<h6>违章记分：</h6>
								<h6>罚款金额：</h6>
								<h6>处理状态：</h6>
							</view>
						</uni-list-item>
					</uni-list>
					<button v-if="dataList.length > 10" type="default" style="margin: 10rpx; background-color: white;"
						@click="showMoreData()">查看更多</button>
					<view v-else style="text-align: center;">
						<view v-if="dataList.length === 0">
							<text class="noMore">暂无查询</text>
						</view>
						<view v-else><text class="noMore">没有更多了</text></view>
					</view>
				</view>
			</view>
			<view v-else style="text-align: center;">
				<text class="noMore">暂无查询结果</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 获取输入框内容
				city: '',
				carNo: '',
				//列表数据
				dataList: [],
				count: 0,
				displayCount: 10,
				visibleCount: 5,
			}
		},
		methods: {
			search() {
				// console.log(this.city+this.carNo)
				this.city = ''
				this.carNo = ''
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/traffic/illegal/list?carnumber' + this.city +
						this.carNo,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					data: {},
					success: res => {
						console.log(this.count)
						this.count += 1
						this.dataList = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			showMoreData() {
				this.visibleCount += 5;
			},
			tapListItem(e) {
				uni.setStorageSync("t_data",e);
				uni.navigateTo({
					url: '../traffic_record/traffic_record',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.search {
		display: flex;
		flex-flow: nowrap;
	}

	.noMore {
		padding: 40rpx;
	}
</style>
