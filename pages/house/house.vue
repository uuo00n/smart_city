<template>
	<view>
		<view class="find-house">
			<view>
				<uni-section title="房源搜索" sub-title="" type="line"></uni-section>
			</view>
			<view class="search-house">
				<uni-search-bar placeholder="请输入你要搜索的房源" @confirm="search" cancelText="搜索" @cancel="search">
				</uni-search-bar>
			</view>
		</view>
		<view class="type-item">
			<view>
				<uni-section title="房源类型" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="4" :showBorder="true" :highlight="true" :square="false" @change="tapTypeItem">
					<uni-grid-item v-for="(item, index) in type" :index="index" :key="index">
						<view class="grid-item-box">
							<text>{{item}}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="body">
			<view>
				<uni-section title="房源" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item v-for="(item,index) in temp_data.slice(0,this.visibleCount)" :key="index"
						:clickable="true" @click="tapListItem(item)">
						<view slot="header" style="width: 250px;">
							<h5>{{item.sourceName}}</h5>
							<h6>位置：{{item.address}}</h6>
							<h6>面积：{{item.areaSize}}㎡</h6>
							<h6>价格：{{item.price}}</h6>
							<h6 style="overflow: hidden; text-overflow: ellipsis;white-space: nowrap;">
								简历：{{item.description}}</h6>
						</view>
						<image :src="'http://124.93.196.45:10001'+item.pic" slot="footer"
							style="width: 200rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
				<button v-if="temp_data.length > 10" type="default" style="margin: 10rpx; background-color: white;"
					@click="showMoreData()">查看更多</button>
				<view v-else style="text-align: center;">
					<view v-if="temp_data.length === 0">
						<text class="noMore">暂无查询</text>
					</view>
					<view v-else><text class="noMore">没有更多了</text></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				displayCount: 10,
				visibleCount: 5,
				temp_data: [],
				type: ['二手', '租房', '楼盘', '中介']
			}
		},
		mounted() {
			this.getAllData()
		},
		methods: {
			showMoreData() {
				this.visibleCount += 5;
			},
			getAllData() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/house/housing/list',
					method: 'GET',
					data: {
						// "pageNum": "",
						// "pageSize": ""
					},
					success: res => {
						this.temp_data = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			search(e) {
				if (e.value === '') {
					uni.showToast({
						title: '搜索内容不能为空',
						position: 'bottom',
						icon: 'none'
					});
				} else {
					let sName = e.value;
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/house/housing/list?sourceName=' + sName,
						method: 'GET',
						data: {},
						header: {
							Authorization: uni.getStorageSync('token')
						},
						success: (res) => {
							this.temp_data = res.data.rows;
							console.log(this.temp_data)
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapTypeItem(e) {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/house/housing/list?houseType=' + this.type[e
						.detail.index],
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.temp_data = res.data.rows;
						console.log(res)

					},
					fail: () => {},
					complete: () => {}
				});

			},
			tapListItem(e) {
				uni.setStorageSync("h_data",e);
				uni.navigateTo({
					url: '../house_about/house_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}

	.noMore {
		padding: 40rpx;
	}
</style>
