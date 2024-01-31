<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in loop" @click="">
					<image :src="host+item.coverImgUrl" mode=""></image>
					<view style="position: absolute; bottom: 0;">
						<text style="color: #FFFFFF;">{{item.name}}</text>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="AreaClass">
			<view>
				<uni-section title="驿站地区" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="3" :showBorder="false" :highlight="false" :square="false">
					<uni-grid-item v-for="(item,index) in AreaClass" :index="index" :key="index">
						<view class="grid-item-box" @click="goClassAbo(item)">
							<image :src="host + item.imgUrl" mode="" class="image"></image>
							<text class="text">{{ item.name }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="StationList">
			<view>
				<uni-section title="青年驿站列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-collapse accordion v-model="accordionVal">
					<view v-for="(item,index) in stationList">
						<uni-list-item clickable @click="" style="display: flex;">
							<image :src="host+item.coverImgUrl" slot="header" mode="aspectFit"
								style="flex: 1; width: 200rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;">
							</image>
							<view slot="body" style="flex: 3;padding: 10rpx;">
								<view>{{item.name}}</view>
								<view style="font-size: 25rpx;">
									<view>剩余床位：男：{{item.bedsCountBoy}}位&nbsp;女：{{item.bedsCountGirl}}位</view>
									<view
										style="width: 250px; white-space: nowrap;overflow: hidden;text-overflow: ellipsis;">
										详细地址：{{item.address}}</view>
								</view>
							</view>
							<view slot="footer" style="display: flex;justify-items: flex-end; align-items: flex-end;">
								<button type="primary" size="mini" @click="introduce(index)">介绍</button>
							</view>
						</uni-list-item>
						<uni-collapse-item title="站点介绍" style="background-color: white;" v-model="introduceSta[index]">
							<view style="padding: 30rpx;">
								<text>{{item.introduce}}</text>
							</view>
						</uni-collapse-item>
					</view>
				</uni-collapse>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				loop: [],
				AreaClass: [],
				stationList: [],
				accordionVal: '1',
				introduceSta:[],
				// introduceSta: Array.from({
				// 	length: this.stationList.length
				// }, () => false)
			}
		},
		mounted() {
			this.getLoopList()
			this.getArea()
			this.getStationList()
		},
		methods: {
			getLoopList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/youth-inn/list?pageNum=1&pageSize=5',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.loop = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getArea() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/talent-policy-area/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.AreaClass = res.data.data
						console.log(this.AreaClass)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassAbo(e) {
				uni.setStorageSync("stationMsg",e)
				uni.navigateTo({
					url: '../station_about/station_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getStationList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/youth-inn/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.stationList = res.data.rows
						console.log(this.stationList)
						this.introduceSta = Array.from({
							length: this.stationList.length
						}, () => false)
						console.log(this.introduceSta)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			introduce(index) {
				console.log(index)
				console.log(this.introduceSta[index])
				this.introduceSta[index] = true;
				console.log(this.introduceSta[index])
			}
		}
	}
</script>

<style>
	.pic-loop image {
		width: 100%;

	}

	.pic-loop swiper {
		height: 240px;
	}

	.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}

	.image {
		width: 70px;
		height: 30px;
	}

	.text {
		font-size: 12px;
		margin-top: 5px;
	}
</style>
