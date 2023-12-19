<template>
	<view>
		<view style="padding: 10rpx;">
			<view>
				<uni-segmented-control :current="current" :values="classItem" @clickItem="onClickItem">
				</uni-segmented-control>
			</view>
			<view class="content">
				<uni-list v-if="listData.length != 0">
					<uni-list-item title="" note="" v-for="(item,index) in listData"></uni-list-item>
				</uni-list>
				<view v-else style="padding: 50rpx; text-align: center;">
					<text>暂无数据</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				classItem: ["已完成", "已报名", "已取消"],
				current: 0,
				listData:[]
			}
		},
		mounted() {
			this.getListData()
		},
		methods: {
			onClickItem(e) {
				if (this.current != e.currentIndex) {
					this.current = e.currentIndex;
				}
				this.getListData(this.current)
			},
			getListData(e) {
				console.log(this.current)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/volunteer-service/activity/my-list/' + e,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.listData = res.data.rows
						console.log(res)
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
