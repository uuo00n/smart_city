<template>
	<view>
		<view>
			<uni-section :title="data.parkName" sub-title="" type="line" class="title"></uni-section>
		</view>
		<view class="bd">
			<view class="">地址：{{ data.address }}</view>
			<view>距离：{{ data.distance }}米</view>
			<view style="">停车场状态：</view>
			<view v-if="data.open === 'Y'">
				<view>
					<view style="margin-left: 36rpx">对外开放</view>
					<view>
						车位信息：
						<view style="margin-left: 36rpx">总停车位：{{ data.allPark }}个</view>
						<view style="margin-left: 36rpx">剩余车位：{{ data.vacancy }}个</view>
					</view>
				</view>
				<view>
					<view>收费标准：{{ data.rates }}元/每小时</view>
					<view>最高封顶：{{ data.priceCaps }}元/天</view>
					
				</view>
			</view>
			<view v-else>不外开放</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			data: []
		};
	},
	mounted() {
		this.getMsg();
	},
	methods: {
		getMsg() {
			let id = uni.getStorageSync('pkMsgid');
			console.log(id);
			uni.request({
				url: 'http://124.93.196.45:10001/prod-api/api/park/lot/' + id,
				method: 'GET',
				data: {},
				success: (res) => {
					this.data = res.data.data;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
};
</script>

<style>
.title {
	font-weight: bold;
}
.bd {
	padding: 30rpx;
}
.ftsz {
	size: 18rpx;
}
</style>
