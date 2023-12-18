<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="news-list">
			<view>
				<uni-section title="新闻列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.title" :note="item.createTime" v-for="(item,index) in newsList"
						clickable>
						<image :src="host+item.imgUrl" slot="header"
							style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="nav">
			<view>
				<uni-section title="活动导航" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="2" :highlight="true" :square="false" clickable @change="goSev">
					<uni-grid-item v-for="(item,index) in adv" :index="index">
						<text class="clsItem">{{item}}</text>
					</uni-grid-item>
				</uni-grid>
			</view>	
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				ad: [],
				newsList: [],
				adv:["志愿活动","我的活动"]
			}
		},
		mounted() {
			this.getRotation()
			this.getNews()
		},
		methods: {
			getRotation() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/ad-banner/list',
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
			getNews() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/news/list?pageSize=5&pageNum=1',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.newsList = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goSev(e) {
				if(e.detail.index === 0){
					uni.navigateTo({
						url: '../volunteer_activities/volunteer_activities',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}else{
					uni.navigateTo({
						url: '../volunteer_my/volunteer_my',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
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
