<template>
	<view>
		<view class="search-body">
			<uni-section title="搜索结果" sub-title="" type="circle"></uni-section>
		</view>
		<view v-if="serList.length != 0">
			<uni-list>
				<uni-list-item v-for="(item,index) in serList" :key="index">
					<view slot="body" style="margin-right: 20%;">
						<view>
							<view>{{item.name}}</view>
							<view style="font-size: 25rpx;">
								<view>从业年限：{{item.workStartAt}}</view>
								<view>咨询人数：{{item.serviceTimes}}人</view>
								<view>法律专长：{{item.legalExpertiseName}}</view>
								<view>好评率：{{item.favorableRate}}%</view>
							</view>
						</view>
					</view>
					<image :src="host+item.avatarUrl" slot="header"
						style="width: 150rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
					<view slot="footer" style="display: flex;justify-items: flex-end; align-items: flex-end;">
						<button type="primary" size="mini">咨询</button>
					</view>
				</uni-list-item>
			</uni-list>
		</view>
		<view v-else style="text-align: center;margin: 100rpx;">
			<text>暂无查询结果</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				serList: []
			}
		},
		mounted() {
			this.SearchLast()
		},
		methods: {
			SearchLast() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list?name=' + uni
						.getStorageSync("lawyerVal"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.serList = res.data.rows
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
