<template>
	<view>
		<view class="all-park">
			<view class="">
				<uni-section title="全部停车场" sub-title="" type="line"></uni-section>
			</view>
			<view class="" style="padding:10rpx 20rpx;">
				<uni-list>
					<uni-list-item clickable @click="tapItem(item)" v-for="(item,index) in sortData" :key="index" :title="item.parkName" :note="'空位数量: '+item.vacancy+'\n'
					+'地址：'+item.address+'\n'+'价格：'+item.rates+'元'+'\n'+'距离：'+item.distance+'米'"></uni-list-item>
				</uni-list>
				<button type="default" style="margin: 10rpx; background-color: white;" @click="showMoreData()">查看更多</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				allPark:[],
				displayCount:5,
				visibleCount:5
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
						this.allPark = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			showMoreData(){
				this.visibleCount += 5;
			},
			tapItem(e){
				uni.setStorageSync("pkMsgid",e.id)
				uni.navigateTo({
					url: '../park_about/park_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onNavigationBarButtonTap(e){
			uni.navigateTo({
				url: '../park_history/park_history',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		},
		computed:{
			sortData(){
				return this.allPark.sort((a,b) => a.distance - b.distance).slice(0,this.visibleCount)
			}
		}
	}
</script>

<style>

</style>
