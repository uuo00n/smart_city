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
		<view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				MyLawDate:[],
				lawyerData: [],
			}
		},
		mounted() {
			this.getMyLawDate()
			this.getLawyerMsg()
		},
		methods: {
			getMyLawDate(){
				this.MyLawDate = uni.getStorageSync("law_data")
			},
			getLawyerMsg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/' + uni.getStorageSync("law_data").legalExpertiseId,
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
