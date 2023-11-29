<template>
	<view>
		<view>
			<uni-section title="订单详细" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<view>乘车人：{{name}}</view>
			<view>联系电话：{{phoneNumber}}</view>
			<view>乘车线路：{{line.name}}</view>
			<view>上车点：{{start}}</view>
			<view>下车点：{{end}}</view>
			<view>乘车日期：{{date}}</view>
		</view>
		<view class="bottom_text">
			<view style="margin: 10rpx;">
				<button type="primary" size="default" @click="sendQs">提交订单</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				line:[],
				name:'',
				phoneNumber:'',
				start:'',
				end:'',
				date:''
			}
		},
		mounted() {
			this.getSend()
		},
		methods: {
			getSend(){
				this.line = uni.getStorageSync("busLine")
				this.name = uni.getStorageSync("userName")
				this.phoneNumber = uni.getStorageSync("userPhone")
				this.start = uni.getStorageSync("start")
				this.end = uni.getStorageSync("end")
				this.date = uni.getStorageSync("busLineDate")
				console.log(this.line)
			},
			sendQs(){
				uni.request({
					url: '',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					method: 'POST',
					data: {
						
					},
					success: res => {
						
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
