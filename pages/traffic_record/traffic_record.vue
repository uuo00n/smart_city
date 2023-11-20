<template>
	<view>
		<view>
			<view class="">
				<view class="mg"><h4>违法时间:{{ data }}</h4></view>
				<view class="mg"><h4>违法地点:{{ data }}㎡</h4></view>
				<view class="mg"><h4>违法行为:{{ data }}</h4></view>
				<view class="mg"><h4>通知书号:{{ data }}</h4></view>
				<view class="mg"><h4>违章记分:{{ data }}</h4></view>
				<view class="mg"><h4>罚款金额:{{ data }}</h4></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data:[]				
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData(){
				this.data = uni.getStorageSync("t_data")
			}
		}
	}
</script>

<style>
.mg{
	margin: 10rpx;
}
</style>
