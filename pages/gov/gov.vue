<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in img" @click="">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="demands">
			<view>
				<uni-section title="市民诉求分类" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<swiper style="height: 400rpx;" :indicator-dots="true">
					<swiper-item>
						<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false"
							@change="goClassList">
							<uni-grid-item v-for="(item, index) in govClass.slice(0,8)" :index="index" :key="index">
								<view class="grid-item-box">
									<image :src="host + item.imgUrl" mode="" class="image"></image>
									<text class="text">{{ item.name }}</text>
								</view>
							</uni-grid-item>
						</uni-grid>
					</swiper-item>
					<swiper-item>
						<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false"
							@change="goClassList2">
							<uni-grid-item v-for="(item, index) in govClass.slice(8,govClass.length)" :index="index"
								:key="index">
								<view class="grid-item-box">
									<image :src="host + item.imgUrl" mode="" class="image"></image>
									<text class="text">{{ item.name }}</text>
								</view>
							</uni-grid-item>
						</uni-grid>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<view>
			<view>
				<uni-section title="我的诉求" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list v-if="mySevList.length != 0">
					<uni-list-item v-for="(item,index) in mySevList">
						<view slot="body">
							<view>{{item.title}}</view>
							<view style="font-size: 25rpx;">
								<view>承办单位：{{item.undertaker}}</view>
								<view>提交时间：{{item.createTime}}</view>
								<view>处理状态：<text v-if="item.state == 0">未处理</text><text v-else>已处理</text></view>
							</view>
						</view>
						<image :src="host+item.imgUrl" slot="header"
							style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
				<view v-else>
					<uni-title title="暂无诉求" align="center" type="h3"></uni-title>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				img: [],
				govClass: [],
				mySevList: []
			}
		},
		mounted() {
			this.getLoopImg()
			this.getClass()
			this.getMySev()
		},
		methods: {
			getLoopImg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/ad-banner/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.img = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getClass() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal-category/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						console.log(res)
						this.govClass = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getMySev() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal/my-list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.mySevList = res.data.rows
						console.log(this.mySevList)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassList(e) {
				uni.setStorageSync("gov_class", this.govClass[e.detail.index])
				console.log(this.govClass[e.detail.index])
				uni.navigateTo({
					url: '../gov_class/gov_class',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassList2(e) {
				const index = e.detail.index + 8
				uni.setStorageSync("gov_class", this.govClass[index])
				console.log(this.govClass[index])
				uni.navigateTo({
					url: '../gov_class/gov_class',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},

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

	.image {
		width: 35px;
		height: 35px;
	}

	.text {
		font-size: 12px;
		margin-top: 5px;
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
</style>
