<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad" @click="goAdv(item)">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view>
			<view class="">
				<uni-section title="垃圾分类" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false">
					<uni-grid-item v-for="(item,index) in allClas" :index="index" :key="index">
						<view class="grid-item-box" @click="goClassAbo(item)">
							<image :src="host + item.imgUrl" mode="" class="image"></image>
							<text class="text">{{ item.name }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="hot_search">
			<view class="">
				<uni-section title="热门搜索" sub-title="" type="line"></uni-section>
			</view>
			<view style="padding: 10rpx;">
				<uni-tag :text="item.keyword" type="primary" v-for="(item,index) in hotObj" style="margin: 10rpx;" @click="goAbo(item)"/>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				ad: [],
				allClas: [],
				hotObj:[]
			}
		},
		mounted() {
			this.getAd();
			this.getClass();
			this.getHotSearch()
		},
		methods: {
			getAd() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/ad-banner/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.ad = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getClass() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-classification/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.allClas = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassAbo(e){
				uni.setStorageSync("clsn",e.name)
				uni.navigateTo({
					url: '../waste_class_about/waste_class_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getHotSearch() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-classification/hot/list?pageNum=1&pageSize=20',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.hotObj = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbo(e){
				console.log(e);
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
		width: 65px;
		height: 70px;
	}

	.text {
		font-size: 12px;
		margin-top: 5px;
	}
</style>
