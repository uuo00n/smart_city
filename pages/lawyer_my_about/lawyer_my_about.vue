<template>
	<view>
		<view>
			<uni-section title="律师信息" sub-title="" type="circle"></uni-section>
		</view>
		<view class="lit-msg">
			<view class="msg-list" style="display: flex;">
				<view style="flex: 1;">
					<image :src="host+lawyerData.avatarUrl" mode="" class="pic"></image>
				</view>
				<view style="flex: 2; padding: 70rpx;">
					<view>姓名：{{lawyerData.name}}</view>
					<view>从业年限：{{lawyerData.workStartAt}}</view>
					<view>咨询人数：{{lawyerData.serviceTimes}}人</view>
					<view>法律专长：{{lawyerData.legalExpertiseName}}</view>
					<view>好评率：{{lawyerData.favorableRate}}%</view>
				</view>
			</view>
		</view>
		<view>
			<uni-section title="我的提问" sub-title="" type="line"></uni-section>
		</view>
		<view style="padding: 20rpx;">
			<view>受理状态：<text v-if="qs.state === 0">未受理</text><text v-else>已受理</text></view>
			<view>问题类型：{{qs.legalExpertiseName}}</view>
			<view>问题描述：{{qs.content}}</view>
			<view>相关图片：</view>
			<view style="text-align: center;">
				<image :src="host+qs.imageUrls" mode="aspectFit"></image>
			</view>
			<view>联系电话：<text v-if="qs.phone != ''">{{qs.phone}}</text><text v-else>暂无</text></view>
		</view>
		<view class="bottom_text" v-if="this.qs.state === 1">
			<button type="primary" size="default" style="margin: 10rpx;" @click="goEvaluate()">评价</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				MyLawDate: [],
				lawyerData: [],
				qs: []
			}
		},
		mounted() {
			this.getMyLawDate()
			this.getLawyerMsg()
		},
		methods: {
			getMyLawDate() {
				this.MyLawDate = uni.getStorageSync("law_data")
				this.getMyLawAbout()
			},
			getLawyerMsg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/' + uni
						.getStorageSync("law_data").legalExpertiseId,
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
			getMyLawAbout() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/' + this
						.MyLawDate.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.qs = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goEvaluate() {
				uni.setStorageSync("lawEva", this.qs.id)
				uni.navigateTo({
					url: '../lawyer_my_evaluate/lawyer_my_evaluate',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
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

	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
