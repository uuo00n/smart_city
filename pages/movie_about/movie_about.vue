<template>
	<view>
		<view class="about" style="display: flex;flex-wrap: nowrap;">
			<view style="flex: 1; text-align: center; padding: 20rpx; border-radius: 15rpx;">
				<image :src="host+movie.cover" mode="aspectFit" style="height: 400rpx; width: 300rpx;border-radius: 15px;"></image>
			</view>
			<view style="flex: 2; padding: 50rpx;">
				<view><h3>{{movie.name}}</h3></view>
				<view style="font-size: 25rpx;">
					<view>上映时间：{{movie.playDate}}</view>
					<view>语言：{{movie.language}}</view>
					<view>时长：{{movie.duration}}分钟</view>
					<view style="display: flex;">评分：<uni-rate :readonly="true" :value="movie.score" size="20"/></view>
					<view>想看人数：{{movie.likeNum}}人</view>
					<view class="text-ellipsis">简历：{{movie.introduction}}</view>
				</view>
			</view>
		</view>
		<view>
			<!-- <view>
				<uni-section title="" sub-title="" type=""></uni-section>
			</view> -->
		</view>
		<view>
			<button type="default" @click="back">返回主页</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				movie:[]
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData(){
				this.movie = uni.getStorageSync("movMsg")
				console.log(this.movie)
			},
			back(){
				uni.navigateBack({
					delta:1
				})
			}
		}
	}
</script>

<style>
.text-ellipsis {  
  display: -webkit-box;  
  -webkit-line-clamp: 5;  
  -webkit-box-orient: vertical;  
  overflow: hidden;  
  text-overflow: ellipsis;  
}
</style>
