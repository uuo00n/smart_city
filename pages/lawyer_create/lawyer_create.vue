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
		<uni-popup ref="popup" type="bottom" background-color="#fff">
			<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false"
				@change="getClassItem">
				<uni-grid-item v-for="(item, index) in lawClass" :index="index" :key="index">
					<view class="grid-item-box">
						<image :src="host + item.imgUrl" mode="" class="image"></image>
						<text class="text">{{ item.name }}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</uni-popup>
		<view style="padding: 10rpx 40rpx;">
			<view>
				<uni-forms-item label="问题描述">
					<uni-easyinput type="textarea" v-model="consultData.content" placeholder="请输入需要咨询的内容" />
				</uni-forms-item>
				<uni-forms-item label="联系电话" >
					<uni-easyinput type="number" v-model="consultData.phone" placeholder="请输入联系电话" />
				</uni-forms-item>
				<uni-forms-item label="问题类型">
					<button type="primary" size="mini" @click="open">请选择</button>
					<text style="padding: 10rpx;" v-if="className != ''">当前选择：{{className}}</text>
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
				lawClass: [],
				lawyerData: [],
				className:'',
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
			this.getAllClass()
		},
		methods: {
			getlawMsg() {
				this.lawMsg = uni.getStorageSync("lawyer_msg")
				this.getLawyerMsg()
			},
			getAllClass() {
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
			open() {
				// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
				this.$refs.popup.open('bottom')
			},
			getClassItem(e){
				this.consultData.legalExpertiseId = this.lawClass[e.detail.index].id
				this.className = this.lawClass[e.detail.index].name
				// 自动关闭pop
				this.$refs.popup.close();
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
		
		.image {
			width: 35px;
			height: 35px;
		}
		
		.text {
			font-size: 12px;
			margin-top: 5px;
		}
</style>
