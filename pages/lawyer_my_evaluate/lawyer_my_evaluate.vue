<template>
	<view>
		<view class="score" style="padding: 20rpx;">
			<uni-section title="请为这次咨询打个分吧" sub-title="" type="line"></uni-section>
			<view style="padding: 0 30rpx;">
				<uni-rate v-model="data.score" @change="" />
			</view>
		</view>
		<view class="content" style="padding: 20rpx;">
			<uni-section title="请为这次咨询评价一下吧" sub-title="" type="line"></uni-section>
			<view style="padding: 0 30rpx;">
				<uni-easyinput type="textarea" v-model="data.evaluate" placeholder="评价一下吧" />
			</view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="send()">提交评价</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: {
					id: 0,
					evaluate: '',
					score: 1
				}
			}
		},
		mounted() {
			this.getId()
		},
		methods: {
			getId() {
				this.data.id = uni.getStorageSync("lawEva")
				console.log(this.data.id)
			},
			send() {
				if(this.data.score < 1){
					uni.showToast({
						title: '不能低于1颗星，请重新打分',
						position: 'bottom',
						icon: 'none',
						duration: 3000
					});
				}else{
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/evaluate',
						method: 'POST',
						header: {
							Authorization: uni.getStorageSync("token")
						},
						data: this.data,
						success: res => {
							if(res.data.code === 200){
								uni.showToast({
									title: "评价成功",
									position: 'bottom',
									duration: 3000,
									success() {
										setTimeout(function(){
											uni.redirectTo({
												url: '../lawyer_my_llist/lawyer_my_llist',
											});
										},3000)
									}
								});
							}else{
								uni.showToast({
									title: res.data.msg,
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
