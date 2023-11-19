<template>
	<view>
		<view class="pic">
			<image :src="this.host + this.data.pic" mode=""></image>
		</view>
		<view class="">
			<view class="mg"><h4>房源名称:{{ data.sourceName }}</h4></view>
			<view class="mg"><h4>建筑面积:{{ data.areaSize }}㎡</h4></view>
			<view class="mg"><h4>房源单价:{{ data.price }}</h4></view>
			<view class="mg"><h4>房源类型:{{ data.houseType }}</h4></view>
			<view class="mg"><h4>房源介绍:{{ data.description }}</h4></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host:'http://124.93.196.45:10001',
				data:[]
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData(){
				console.log(this.host)
				this.data = uni.getStorageSync("h_data")
			}
		}
	}
</script>

<style>
.pic image{
	width: 100%;
}

.mg{
	margin: 10rpx;
}
</style>
