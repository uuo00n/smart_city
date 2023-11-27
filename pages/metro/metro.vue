<template>
	<view>
		<view class="header">
			<uni-section :title="'当前位置：'+address" sub-title="" type="circle"></uni-section>
		</view>
		<view>
			<uni-section title="最近车次" :sub-title="'离我最近的车站：'+address" type="line"></uni-section>
		</view>
		<view class="body">
			<uni-card :title="item.lineName" :extra="item.reachTime === 0 ? '已到站' : '到达本站时长：'+item.reachTime+'分钟'"
				v-for="(item,index) in address_met" @click="goAbout(item)">
				<uni-steps
					:options="[{title: item.preStep.name}, {title: item.currentName}, {title: item.nextStep.name}]"
					:active="1" active-color="blue" />
			</uni-card>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				address: '建国门',
				address_met: []
			};
		},
		mounted() {
			this.getMetro()
		},
		methods: {
			// 获取当前地址附近的地铁站
			getMetro() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/metro/list?currentName=' + this.address,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.address_met = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(item){
				uni.setStorageSync("met_msg",item)
				uni.navigateTo({
					url: '../metro_about/metro_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	};
</script>

<style></style>
