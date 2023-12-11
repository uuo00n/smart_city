<template>
	<view>
		<view class="title">
			<view>
				<uni-section title="状态筛选" sub-title="" type="circle"></uni-section>
			</view>
			<view>
				<uni-data-select v-model="sevValue" :localdata="sevAt" @change="sort" :clear="false">
				</uni-data-select>
			</view>
		</view>
		<view class="body">
			<view>
				<uni-section title="筛选结果" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item title="" note="" v-for="(item,index) in MyList" @click="get">
						<view slot="body" style="margin-right: 20%;">
							<view>
								<view style="font-size: 25rpx;">
									<view>律师姓名：{{item.lawyerName}}</view>
									<view>咨询类型：{{item.legalExpertiseName}}</view>
									<view>咨询内容：{{item.content}}</view>
									<view>受理状态：<text v-if="item.state == 0">未受理</text><text v-else>已完成</text></view>
									<view>提交时间：{{item.createTime.slice(0,10)}}</view>
								</view>
							</view>
						</view>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				MyList: [],
				sevValue:0,
				sevAt: [{
						value: 0,
						text: "未受理",
					}, {
						value: 1,
						text: "已完成",
					}
				]
			}
		},
		mounted() {
			this.getMyLawList()
		},
		methods: {
			getMyLawList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.MyList = res.data.rows
						console.log(this.MyList)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			sort(e){
				console.log(this.sevAt[e].value);
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/list?state='+ this.sevAt[e].value,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.MyList = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getListItem(e){
				console.log(e)
			}
		}
	}
</script>

<style>

</style>
