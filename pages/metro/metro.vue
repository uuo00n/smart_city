<template>
	<view>
		<!-- 当前位置为{{this.res.address.city}} -->
	</view>
</template>

<script>
export default {
	data() {
		return {
			
		};
	},
	mounted() {
		this.getlocal();
	},
	methods: {
		getlocal() {
			uni.getLocation({
				type: 'wgs84',
				geocode:true,
				highAccuracyExpireTime:5000,
				success: res => {
					console.log(res.latitude);
					console.log(res.longitude);
					console.log(res.address);
				}
			});
		}
	}
};
</script>

<style></style>
