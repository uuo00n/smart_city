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
			<view v-if="MyList.length != 0">
				<uni-list>
					<uni-list-item title="" note="" v-for="(item,index) in MyList" @click="getListItem(item)" clickable>
						<view slot="body">
							<view>
								<view style="font-size: 25rpx;">
									<view>律师姓名：{{item.lawyerName}}</view>
									<view>法律专长：{{item.legalExpertiseName}}</view>
									<view>咨询内容：{{item.content}}</view>
									<view>受理状态：<text v-if="item.state == 0">未受理</text><text v-else>已完成</text></view>
									<view>提交时间：{{item.createTime.slice(0,10)}}</view>
								</view>
							</view>
						</view>
						<image :src="host+item.imageUrls" slot="header"
							style="width: 200rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
			<view v-else style="text-align: center;">
				<text>暂无查询结果</text>
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
						console.log(res)
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
				uni.setStorageSync("law_data",e);
				uni.navigateTo({
					url: '../lawyer_my_about/lawyer_my_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
.pic {
		padding: 60rpx;
		width: 150rpx;
		height: 200rpx;
		border-radius: 5px;
		margin-right: 10px;
	}
</style>
