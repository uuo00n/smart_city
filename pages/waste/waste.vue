<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad" @click="goAdv(item)">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="waste-nav">
			<view>
				<uni-section title="活动导航" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="2" :highlight="true" :square="false" clickable @change="goSev">
					<uni-grid-item v-for="(item,index) in nav" :index="index">
						<text class="clsItem">{{item}}</text>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="">
			<view>
				<uni-section title="新闻" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<view class="uni-padding-wrap uni-common-mt" style="padding:0rpx 20rpx;">
					<uni-segmented-control :values="newClass" @clickItem="getNewsData" />
				</view>
				<view class="news-list" style="padding: 20rpx;">
					<uni-list>
						<uni-list-item v-for="(item,index) in newsData" :key="index" clickable @click="goNewsAbo(item)">
							<view slot="body">
								<view>{{item.title}}</view>
								<view style="font-size: 25rpx;">
									<view>{{item.createTime}}</view>
								</view>
							</view>
							<image :src="host+item.imgUrl" slot="header"
								style="width: 250rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
						</uni-list-item>
					</uni-list>
				</view>
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
				nav:["搜索","分类"],
				newClass:["新时尚","党员在行动","分类达人","社区动态"],
				newsData:[],
				allClass:[],
				newsData:[]
			}
		},
		mounted() {
			this.getAd()
			this.getClass()
			this.getnNew()
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
			getClass(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news-type/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.allClass = res.data.rows
						console.log(this.allClass)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goSev(e){
				const nav = e.detail.index
				if(nav == 0){
					uni.navigateTo({
						url: '../waste_search/waste_search',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}else{
					uni.navigateTo({
						url: '../waste_class/waste_class',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			getnNew(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news/list?type='+7,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.newsData = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getNewsData(e){
				var clsId = this.allClass[e.currentIndex].id
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news/list?type='+clsId,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.newsData = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goNewsAbo(e){
				console.log(e)
				uni.setStorageSync("waste_news",e)
				uni.navigateTo({
					url: '../waste_news/waste_news',
					success: res => {},
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
	
	.clsItem {
		text-align: center;
		padding: 40rpx;
	}
</style>
