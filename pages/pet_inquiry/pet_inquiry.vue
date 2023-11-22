<template>
	<view>
		<view class="doc_data">
			<view style="flex: 1;">
				<image :src="host+doc_data.avatar" mode="" class="pic"></image>
			</view>
			<view style="flex: 2; padding: 70rpx;">
				<view>医生姓名：{{doc_data.name}}</view>
				<view>医生职称：{{doc_data.jobName}}</view>
				<view>执业编号：{{doc_data.practiceNo}}</view>
			</view>
		</view>
		<view class="question_list">
			<view>
				<uni-section title="问题列表" sub-title="" type="line"></uni-section>
			</view>
			<view style="padding-bottom: 150rpx;">
				<uni-list>
					<uni-list-item :title="item.question" :note="item.createTime" v-for="(item,index) in question_data" @click="goAbout(item)" clickable></uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="bottom_text">
			<view style="flex: 3;"><input type="text" value="" v-model="inputValue" ref="inputElement"
					placeholder="请输入要提问的内容" @input="onKeyInput" /></view>
			<view style="flex: 1; text-align: center;"><button class="mini-btn" type="primary" size="mini"
					@click="sendQuestion">发送</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				doc_data: [],
				inputValue: '',
				question_data: []
			}
		},
		mounted() {
			this.getDoc()
			this.getQuestion()
		},
		methods: {
			getDoc() {
				this.doc_data = uni.getStorageSync("doc_msg")
			},
			onKeyInput: function(event) {
				this.inputValue = event.target.value
			},
			getQuestion() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/inquiry/list?doctorId=' + this
						.doc_data.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.question_data = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			sendQuestion() {
				console.log(this.inputValue)
				if (this.inputValue != '') {
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/inquiry',
						method: 'POST',
						header: {
							Authorization: uni.getStorageSync('token')
						},
						data: {
							"doctorId": this.doc_data.id,
							"question": this.inputValue
						},
						success: res => {
							uni.showToast({
								title: '提交问诊成功',
								position: 'bottom',
								icon: 'none'
							});
							this.getQuestion()
							this.inputValue = ''
							this.$refs.inputElement.value = ''
							setTimeout(() => {
								uni.navigateBack({
									delta: 1,
								});
							}, 3000);
						},
						fail: () => {},
						complete: () => {}
					});
				} else {
					uni.showToast({
						title: '问诊内容不能为空',
						position: 'bottom',
						icon: 'none'
					});
				}
			},
			goAbout(item){
				console.log(item)
				uni.setStorageSync("q_data",item)
				uni.navigateTo({
					url: '../pet_about/pet_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.doc_data {
		display: flex;
		flex-wrap: nowrap;
	}

	.pic {
		padding: 60rpx;
		width: 200rpx;
		height: 200rpx;
		border-radius: 5px;
		margin-right: 10px;
	}

	.bottom_text {
		/* width: 100vw;
		padding: 6px 12px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #ededf4;
		position: absolute; */

		width: 100vw;
		padding: 6px 12px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #ededf4;
		position: fixed;
		bottom: 0;
		/* 将元素定位到页面底部 */
	}
</style>
