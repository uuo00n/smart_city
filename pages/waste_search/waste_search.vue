<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad" @click="goAdv(item)">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="search_bar">
			<view class="">
				<uni-section title="垃圾搜索" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-search-bar cancelText="搜索" @cancel="goSearch" placeholder="请输入你要搜索的分类"></uni-search-bar>
			</view>
		</view>
		<view class="hot_search">
			<view class="">
				<uni-section title="热门搜索" sub-title="" type="line"></uni-section>
			</view>
			<view style="padding: 10rpx;">
				<uni-tag :text="item.keyword" type="primary" v-for="(item,index) in hotObj" style="margin: 10rpx;" />
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
				adObj: [],
				hotObj: []
			}
		},
		mounted() {
			this.getAd()
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
			goSearch(e) {
				uni.setStorageSync("sb", e.value)
				uni.navigateTo({
					url: '../waste_search_obj/waste_search_obj',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getHotSearch() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-classification/hot/list?pageNum=1&pageSize=10',
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
</style>
