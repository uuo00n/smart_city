<template>
	<view>
		<view>
			<uni-section title="法律咨询" sub-title="" type="line"></uni-section>
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
			<uni-section title="问题信息" sub-title="" type="circle"></uni-section>
		</view>
		<view style="padding: 10rpx 40rpx;">
			<view>
				<uni-forms-item label="问题描述">
					<uni-easyinput type="textarea" v-model="consultData.content" placeholder="请输入需要咨询的内容" />
				</uni-forms-item>
			</view>
			<view class="example-body">
				<uni-file-picker limit="1" title="请选择需要上传的图片" @success="sendImg"></uni-file-picker>
			</view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="sendQs">创建咨询</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				lawMsg: [],
				lawyerData: [],
				consultData: {
					lawyerId: '',
					legalExpertiseId: '',
					content: '',
					imageUrls: '',
					phone: ''
				},
				IMGsj: [],
			}
		},
		mounted() {
			this.getlawMsg()
		},
		methods: {
			getlawMsg() {
				this.lawMsg = uni.getStorageSync("lawyer_msg")
				this.getLawyerMsg()
			},
			getLawyerMsg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/' + this.lawMsg.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.lawyerData = res.data.data
						console.log(this.lawMsg);
						this.consultData.lawyerId = this.lawMsg.id
						this.consultData.legalExpertiseId = this.lawMsg.legalExpertiseId
					},
					fail: () => {},
					complete: () => {}
				});
			},
			sendImg(e) {
				const TF = e.tempFiles[0];
				console.log(TF)
				uni.uploadFile({
					url: 'http://124.93.196.45:10001/prod-api/common/upload',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					filePath: TF.image.location,
					success: (res) => {
						if(res.statusCode === 200){
							this.IMGsj = JSON.parse(res.data)
							this.consultData.imageUrls = "/prod-api" + this.IMGsj.fileName
						}else{
							uni.showToast({
								title: '上传图片失败 ',
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}
					}
				})
			},
			sendQs() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice',
					method: 'POST',
					data: this.consultData,
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						console.log(res)
						if(res.data.code === 200){
							uni.showToast({
								title: '创建咨询成功 ',
								position: 'bottom',
								duration: 3000,
								success() {
									setTimeout(function(){
										uni.redirectTo({
											url: '../lawyer/lawyer',
										});
									},3000)
								}
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// getSendState(){
				
			// }
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

	.bd-pad {
		padding: 20rpx;
	}

	.header-img {
		height: 80rpx;
		width: 80rpx;
		margin: 10rpx;
	}

	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
