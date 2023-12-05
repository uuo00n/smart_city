<template>
	<view>
		<view class="title">
			<uni-section title="律师介绍" sub-title="" type="line"></uni-section>
		</view>
		<view class="lit-msg">
			<view class="msg-list" style="display: flex;">
				<view style="flex: 1;">
					<image :src="host+lawyerData.avatarUrl" mode="" class="pic"></image>
				</view>
				<view style="flex: 2; padding: 70rpx;">
					<view>法律专长：{{lawyerData.legalExpertiseName}}</view>
					<view>咨询人数：{{lawyerData.serviceTimes}}人</view>
					<view>服务次数：{{lawyerData.favorableCount}}次</view>
				</view>
			</view>
		</view>
		<view style="padding: 10rpx;">
			<uni-segmented-control :current="current" :values="ItemClass" @clickItem="changeItem" styleType="text">
			</uni-segmented-control>
			<view style="padding: 10rpx;">
				<view v-show="current === 0">
					律师基本信息
				</view>
				<view v-show="current === 1">
					用户评论
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				lawyerData: [],
				ItemClass: ['律师基本信息', '用户评论'],
				current: 0
			}
		},
		mounted() {
			this.getLawyerMsg()
		},
		methods: {
			getLawyerMsg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/' + uni
						.getStorageSync("lawyer_msg").id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.lawyerData = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			changeItem(e) {
				if (this.current != e.currentIndex) {
					this.current = e.currentIndex;
				}
			}
		}
	}
</script>

<style>
	.pic {
		padding: 60rpx;
		width: 150rpx;
		height: 200rpx;
		border-radius: 5px;
		margin-right: 10px;
	}
</style>
