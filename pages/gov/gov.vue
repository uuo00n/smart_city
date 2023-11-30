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
						<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false" @change="">
							<uni-grid-item v-for="(item, index) in govClass.slice(0,8)" :index="index" :key="index">
								<view class="grid-item-box">
									<image :src="host + item.imgUrl" mode="" class="image"></image>
									<text class="text">{{ item.name }}</text>
								</view>
							</uni-grid-item>
						</uni-grid>
					</swiper-item>
					<swiper-item>
						<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false" @change="">
							<uni-grid-item v-for="(item, index) in govClass.slice(8,govClass.length)" :index="index" :key="index">
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
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				img: [],
				govClass:[]
			}
		},
		mounted() {
			this.getLoopImg()
			this.getClass()
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
						console.log(this.img)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getClass(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal-category/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.govClass = res.data.rows
						console.log(this.govClass)
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
