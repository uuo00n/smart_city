<template>
	<view>
		<image :src="host+url" mode="" style="width: 100%;"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host:'http://124.93.196.45:10001',
				url:''
			}
		},
		mounted() {
			this.getcityLine()
		},
		methods: {
			getcityLine(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/metro/city',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.url = res.data.data.imgUrl
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
