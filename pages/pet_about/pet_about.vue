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
				<uni-section title="问题详细" sub-title="" type="line"></uni-section>
			</view>
			<view style="padding-bottom: 150rpx;" v-if="dialogue.length != 0">
				<uni-list>
					<uni-list-item title="" note=""></uni-list-item>
				</uni-list>
			</view>
			<view v-else style="padding: 100rpx; text-align: center;">
				暂无对话
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
				q_data:[],
				inputValue: '',
				dialogue:[]
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData() {
				this.doc_data = uni.getStorageSync("doc_msg")
				this.q_data = uni.getStorageSync("q_data")
				// console.log(this.doc_data)
				console.log(this.q_data)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/inquiry-message/list?inquiryId='+this.q_data.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.dialogue = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			onKeyInput: function(event) {
				this.inputValue = event.target.value
			},
			
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
