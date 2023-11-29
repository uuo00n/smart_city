<template>
	<view>
		<view>
			<uni-section title="请选择乘车日期" sub-title="" type="line"></uni-section>
		</view>
		<view class="pick-date" style="margin: 10rpx;">
			<uni-datetime-picker type="date" :clear-icon="false" v-model="single"/>
		</view>
		<view style="margin: 50rpx;" v-if="single != ''">
			<uni-title :title="'当前选中乘车日期为：'+single" type="h4" align="center"></uni-title>
		</view>
		<view class="bottom_text">
			<view style="margin: 10rpx; display: flex;">
				<button type="primary" size="default"  @click="previousView">上一页</button>
				<button type="primary" size="default"  @click="nextView">下一页</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				single: ''
			}
		},
		methods: {
			previousView(){
				uni.navigateBack({
					delta: 1
				});
			},
			nextView(){
				if(this.single != ''){
					uni.setStorageSync("busLineDate",this.single)
					console.log(this.single)
					uni.navigateTo({
						url: '../bus_passenger/bus_passenger',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}else{
					uni.showToast({
						title: '输入框不能为空',
						position: 'bottom',
						icon: 'none',
						duration: 3000
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
