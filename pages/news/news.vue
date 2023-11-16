<template>
	<view>
		<view class="news-loop">
			
		</view>
		<view class="news-class">
			
		</view>
		<view class="news-list">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		mounted() {
			this.getData();
		},
		methods: {
			getData(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/park/lot/list',
					method: 'GET',
					data: {},
					success: res => {
						console.log(res.data.rows)
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
