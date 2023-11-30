<template>
	<view>
		<view class="">
			<view>
				<uni-section title="乘车人信息" sub-title="" type="circle"></uni-section>
			</view>
			<view style="padding: 10rpx 50rpx;">
				乘车人：<input type="text" v-model="Uname" placeholder="请输入姓名" class="input" />
			</view>
			<view style="padding: 10rpx 50rpx;">
				联系电话：<input type="number" v-model="Unumber" placeholder="请输入姓名" class="input" />
			</view>
		</view>
		<view class="bus-line">
			<view>
				<uni-section title="乘车区间" sub-title="" type="circle"></uni-section>
			</view>
			<view style="display: flex;padding: 15rpx;">
				<view style="flex: 2;">
					<uni-data-select :value="start" :localdata="steps" @change="getStart" placeholder="请选择始发站"
						:clear="false"></uni-data-select>
				</view>
				<view style="flex: 1; text-align: center;">
					→
				</view>
				<view style="flex: 2;">
					<uni-data-select :value="end" :localdata="steps" @change="getEnd" placeholder="请选择终点站"
						:clear="false"></uni-data-select>
				</view>
			</view>
		</view>
		<view class="bottom_text">
			<view style="margin: 10rpx;">
				<button type="primary" size="default" @click="goSend()">下一步</button>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				line: [],
				steps: [],
				start: 0,
				end: 0,
				startStep: '',
				endStep: '',
				Uname: '',
				Unumber: ''
			}
		},
		mounted() {
			this.getStep()
		},
		methods: {
			getStep() {
				this.line = uni.getStorageSync("busLine")
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/bus/stop/list?linesId=' + this.line.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.steps = res.data.rows.map(item => ({
							value: item.stepsId,
							text: item.name
						}))
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getStart(e) {
				const index = e - 1
				this.startStep = this.steps[index].text
			},
			getEnd(e) {
				const index = e - 1
				this.endStep = this.steps[index].text
			},
			goSend() {
				uni.setStorageSync("userName", this.Uname)
				uni.setStorageSync("userPhone", this.Unumber)
				uni.setStorageSync("start", this.startStep)
				uni.setStorageSync("end", this.endStep)
				console.log(uni.getStorageSync("start"))
				console.log(uni.getStorageSync("end"))
				uni.navigateTo({
					url: '../bus_send/bus_send',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.input {
		margin: 5rpx;
		padding: 10rpx;
		border-radius: 15rpx;
		border: 1px solid;
	}

	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
