<template>
	<view>
		<view class="pic-loop">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item v-for="(item,index) in aboMsg.imageUrlList" @click="">
					<image :src="host+item" mode=""></image>
					<view style="position: absolute; bottom: 0;">
						<text style="color: #FFFFFF;">{{item.name}}</text>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="body">
			<uni-title :title="aboMsg.name" type="h1" style="padding: 0rpx 20rpx;"></uni-title>
			<view style="padding:0rpx 30rpx;">
				<view>地址：{{aboMsg.address}}</view>
				<view>联系电话：{{aboMsg.phone}}</view>
				<view>入住时间：{{aboMsg.workTime}}</view>
				<view>剩余床位：男：{{aboMsg.bedsCountBoy}}位&nbsp;女：{{aboMsg.bedsCountGirl}}位</view>
			</view>
		</view>
		<view class="footer">
			<view class="">
				<view>
					<uni-section title="驿站介绍" sub-title="" type="circle"></uni-section>
				</view>
				<view style="padding: 10rpx 30rpx;">
					{{aboMsg.introduce}}
				</view>
			</view>
			<view class="">
				<view>
					<uni-section title="内部配置" sub-title="" type="circle"></uni-section>
				</view>
				<view style="padding: 10rpx 30rpx;">
					{{aboMsg.internalFacilities}}
				</view>
			</view>
			<view class="">
				<view>
					<uni-section title="周边配套" sub-title="" type="circle"></uni-section>
				</view>
				<view style="padding: 10rpx 30rpx;">
					{{aboMsg.surroundingFacilities}}
				</view>
			</view>
			<view class="">
				<view>
					<uni-section title="特色服务" sub-title="" type="circle"></uni-section>
				</view>
				<view style="padding: 10rpx 30rpx;">
					{{aboMsg.specialService}}
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
				aboMsg: []
			}
		},
		mounted() {
			this.getAbo()
		},
		methods: {
			getAbo() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/youth-inn/youth-inn/' + uni.getStorageSync(
						"stationMsgAbo").id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.aboMsg = res.data.data
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
