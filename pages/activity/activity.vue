<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in ad">
					<image :src="host+item.advImg" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="act_class">
			<view>
				<uni-section title="活动分类" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="3" :showBorder="false" :highlight="false" :square="false" @change="">
					<uni-grid-item v-for="(item, index) in atcClass" :index="index" :key="index">
						<view class="grid-item-box">
							<text class="text">{{ item.name }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="act_list">
			<view>
				<uni-section title="活动列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.name" :note="'报名人数:'+item.signupNum+'人'+' '+'点赞数'+item.likeNum+'人'"
						clickable v-for="(item,index) in atcList" @click="">
						<image :src="host+item.imgUrl" slot="header"
							style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
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
				atcClass: [],
				atcList: []
			}
		},
		mounted() {
			this.getRotation()
			this.getActclass()
			this.getActList()
		},
		methods: {
			getRotation() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/rotation/list',
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
			getActclass() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/category/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.atcClass = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getActList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/activity/list',
					method: 'GET',
					data: {},
					success: res => {
						this.atcList = res.data.rows
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
		height: 200px;
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
