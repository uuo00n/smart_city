<template>
	<view>
		<view>
			<uni-section title="诉求创建" sub-title="" type="circle"></uni-section>
		</view>
		<view>
			<uni-forms ref="baseForm" v-model="formData">
				<uni-forms-item label="诉求标题">
					<uni-easyinput v-model="formData.title" placeholder="请输入诉求标题" />
				</uni-forms-item>
				<uni-forms-item label="诉求内容">
					<uni-easyinput type="textarea" v-model="formData.content" placeholder="请输入诉求内容" />
				</uni-forms-item>
				<uni-forms-item label="诉求单位" v-model="formData.undertaker">
					<uni-easyinput v-model="formData.undertaker" placeholder="请输入诉求单位" />
				</uni-forms-item>
			</uni-forms>
		</view>
		<view style="padding: 20rpx;">
			<view class="example-body">
				<uni-file-picker limit="1" title="请选择需要上传的图片" @success="sendImg"></uni-file-picker>
			</view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="send">新建诉求</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formData: {
					appealCategoryId: 0,
					title: '',
					content: '',
					undertaker: '',
					imgUrl: ''
				}
			}
		},
		mounted() {
			this.getClassId()
		},
		methods: {
			getClassId() {
				this.formData.appealCategoryId = uni.getStorageSync("gov_class").id
			},
			sendImg(e) {
				console.log(e.tempFiles)
				// uni.uploadFile({
				// 	url: 'http://124.93.196.45:10001/prod-api/common/upload', 
				// 	filePath: tempFilePaths[0],
				// 	name: 'file',
				// 	formData: {
				// 		'user': 'test'
				// 	},
				// 	success: (uploadFileRes) => {
				// 		console.log(uploadFileRes.data);
				// 	}
				// });
			},
			send() {
				console.log(this.formData)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal',
					method: 'POST',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: this.formData,
					success: res => {
						if (res.data.code == 200) {
							uni.showToast({
								title: '提交诉求成功',
								position: 'center',
								duration: 3000,
								success() {
									setTimeout(function() {
										uni.redirectTo({
											url: '../gov_class/gov_class'
										});
									}, 3000);
								}
							});
						} else {
							uni.showToast({
								title: '提交失败 ' + res.data.msg,
								position: 'bottom',
								icon: 'none',
								duration: 3000
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
	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
