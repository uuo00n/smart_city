<template>
	<view>
		<view>
			<uni-list>
				<uni-list-item v-for="(item,index) in lib">
					<view slot="body" @click="goAbout(item)">
						<view>
							<view>{{item.name}}</view>
							<view style="font-size: 25rpx;">
								<view>地址：{{item.address}}</view>
								<view>营业时间：{{item.businessHours}}</view>
								<view>营业状态：<text v-if="item.businessState ==1 " style="color: #18BC37;">营业中</text><text v-else style="color: #E43D33;">休息中</text>
								</view>
							</view>
						</view>
					</view>
				</uni-list-item>
			</uni-list>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lib: []
			}
		},
		mounted() {
			this.getList()
		},
		methods: {
			getList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/digital-library/library/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.lib = res.data.rows
						console.log(this.lib)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(e){
				uni.setStorageSync("lib_id",e.id)
				uni.navigateTo({
					url: '../library_about/library_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
