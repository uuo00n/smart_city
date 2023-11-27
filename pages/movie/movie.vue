<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad" @click="goAdv(item)">
					<image :src="host+item.advImg" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="search-movie">
			<view>
				<uni-section title="影片搜索" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<view class="search-box">
					<uni-search-bar @confirm="Search" cancelText="取消" @cancel="outSearch" @input="search"></uni-search-bar>
				</view>
			</view>
		</view>
		<view v-if="inSearch === true">
			<view><uni-section :title="'搜索结果'" sub-title="" type=""></uni-section></view>
			<view v-if="scList.length === 0" style="text-align: center;margin: 30rpx;">
				<text>暂无查询结果</text>
			</view>
			<view v-else>
				<uni-list>
					<uni-list-item v-for="(item,index) in scList" :key="index">
						<view slot="body">
							<view>{{item.name}}</view>
							<view style="font-size: 25rpx;">
								<view>上映时间：{{item.playDate}}</view>
								<view>语言：{{item.language}}</view>
								<view>时长：{{item.duration}}分钟</view>
								<view style="display: flex;">评分：<uni-rate :readonly="true" :value="item.score" size="20"/></view>
							</view>
						</view>
						<image :src="host+item.cover" slot="header"
							style="width: 150rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="movie-list">
			<view>
				<uni-section title="热门影片" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item v-for="(item,index) in mviList.slice(0,displayCount)" :key="index" clickable @click="goAbout(item)">
						<view slot="body">
							<view>{{item.name}}</view>
							<view style="font-size: 25rpx;">
								<view>上映时间：{{item.playDate}}</view>
								<view>语言：{{item.language}}</view>
								<view>时长：{{item.duration}}分钟</view>
								<view style="display: flex;">评分：<uni-rate :readonly="true" :value="item.score" size="20"/></view>
							</view>
						</view>
						<image :src="host+item.cover" slot="header"
							style="width: 150rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
			<view>
				<button type="default" @click="showAllMov" v-if="displayCount != mviList.length">查看更多</button>
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
				mviList:[],
				scList:[],
				displayCount:5,
				inSearch:false
			}
		},
		mounted() {
			this.getRotation()
			this.getMovieList()
		},
		methods: {
			getRotation() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/movie/rotation/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.ad = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getMovieList(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/movie/film/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.mviList = res.data.rows
						console.log(this.mviList)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			showAllMov(){
				this.displayCount = this.mviList.length
			},
			search(e) {
				this.inSearch = true
				if(e != ''){
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/movie/film/list?name='+e,
						method: 'GET',
						data: {},
						header: {
							Authorization: uni.getStorageSync("token")
						},
						success: res => {
							this.scList = res.data.rows
							console.log(res)
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			outSearch(){
				this.inSearch = false
			},
			goAbout(item){
				uni.setStorageSync("movMsg",item)
				uni.navigateTo({
					url: '../movie_about/movie_about',
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
</style>
