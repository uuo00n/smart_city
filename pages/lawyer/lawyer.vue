<template>
	<view>
		<view class="search-lawyer">
			<view>
				<uni-section title="律师搜索" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<view class="search-box">
					<uni-search-bar  cancelText="搜索" @cancel="goSearch" ></uni-search-bar>
				</view>
			</view>
		</view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in img" @click="">
					<image :src="host+item.imgUrl" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="lawyer-class">
			<view>
				<uni-section title="法律专长" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<swiper style="height: 400rpx;" :indicator-dots="true">
					<swiper-item>
						<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false"
							@change="goClassList">
							<uni-grid-item v-for="(item, index) in lawClass.slice(0,8)" :index="index" :key="index">
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
							<uni-grid-item v-for="(item, index) in lawClass.slice(8,lawClass.length)" :index="index"
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
		<view class="my-legal">
			<view>
				<uni-section title="我的咨询" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<view v-if="myLegalList.length != 0">

				</view>
				<view v-else style="text-align: center;margin: 100rpx;">
					<text>暂无查询结果</text>
				</view>
			</view>
		</view>
		<view class="good-lawyer">
			<view>
				<uni-section title="本月优质律师" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item v-for="(item,index) in topLow" :key="index">
						<view slot="body" style="margin-right: 20%;">
							<view>
								<view>{{item.name}}</view>
								<view style="font-size: 25rpx;">
									<view>从业年限：{{item.workStartAt}}</view>
									<view>咨询人数：{{item.serviceTimes}}人</view>
									<view>法律专长：{{item.legalExpertiseName}}</view>
									<view>好评率：{{item.favorableRate}}%</view>
								</view>
							</view>	
						</view>
						<image :src="host+item.avatarUrl" slot="header"
							style="width: 150rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
						<view slot="footer"  style="display: flex;justify-items: flex-end; align-items: flex-end;">
							<button type="primary" size="mini" @click="goAbout(item)">咨询</button>
						</view>
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
				img: [],
				lawClass: [],
				myLegalList: [],
				topLow: []
			}
		},
		mounted() {
			this.getLoopImg()
			this.getClass()
			this.getMyLegal()
			this.getTopLaw()
		},
		methods: {
			getLoopImg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/ad-banner/list',
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
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-expertise/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.lawClass = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassList(e) {
				uni.setStorageSync("law_class", this.lawClass[e.detail.index])
				uni.navigateTo({
					url: '../lawyer_class/lawyer_class',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			goClassList2(e) {
				const index = e.detail.index + 8
				uni.setStorageSync("law_class", this.lawClass[index])
				uni.navigateTo({
					url: '../lawyer_class/lawyer_class',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getMyLegal() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.myLegalList = res.data.rows
						console.log(this.myLegalList)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getTopLaw() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list-top10',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.topLow = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goSearch(e){
				uni.setStorageSync("lawyerVal",e.value)
				uni.navigateTo({
					url: '../lawyer_search/lawyer_search',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(e){
				uni.setStorageSync("lawyer_msg",e)
				uni.navigateTo({
					url: '../lawyer_about/lawyer_about',
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
