<template>
	<view>
		<view class="head" style="display: flex; flex-wrap: nowrap;">
			<view class="px" style="flex: 1;">
				<view>
					<uni-section title="排序方式" sub-title="" type="circle"></uni-section>
				</view>
				<view style="padding: 20rpx;">
					<uni-data-select v-model="sevValue" :localdata="sevAt" @change="sort" :clear="false">
					</uni-data-select>
				</view>
			</view>
			<view class="sx" style="flex: 1;">
				<view>
					<uni-section title="类型筛选" sub-title="" type="circle"></uni-section>
				</view>
				<view style="text-align: center;margin-top: 5%;">
					<button type="primary" size="mini" @click="open">筛选</button>
				</view>
			</view>
		</view>
		<uni-popup ref="popup" type="bottom" background-color="#fff">
			<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false"
				@change="getNewClassList">
				<uni-grid-item v-for="(item, index) in lawClass" :index="index" :key="index">
					<view class="grid-item-box">
						<image :src="host + item.imgUrl" mode="" class="image"></image>
						<text class="text">{{ item.name }}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</uni-popup>
		<view>
			<view>
				<uni-section title="专长列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item v-for="(item,index) in listData" :key="index">
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
							<button type="primary" size="mini" @click="goAbout(item)">咨询</button>
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
				classData: [],
				classId:0,
				listData: [],
				lawClass: [],
				sevValue: 0,
				sevAt: [{
						value: 0,
						text: "默认排序",
					}, {
						value: 1,
						text: "从业年限",
						at: "workStartAt"
					},
					{
						value: 2,
						text: "服务人数",
						at: "serviceTimes"
					},
					{
						value: 3,
						text: "好评率",
						at: "favorableRate"
					},
				]
			}
		},
		mounted() {
			this.getClass()
			this.getAllClass()
		},
		methods: {
			getClass() {
				this.classData = uni.getStorageSync("law_class")
				this.classId = uni.getStorageSync("law_class").id
				console.log(this.classId)
				this.getList()
			},
			getAllClass() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-expertise/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.lawClass = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list?legalExpertiseId=' +
						this.classId,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.listData = res.data.rows
						console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(e) {
				uni.setStorageSync("lawyer_msg", e)
				uni.navigateTo({
					url: '../lawyer_about/lawyer_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			sort(e) {
				if (e == 0) {
					this.getList()
				} else {
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list?legalExpertiseId=' +
							this.classId + '&sort=' + this.sevAt[e].at,
						method: 'GET',
						data: {},
						header: {
							Authorization: uni.getStorageSync("token")
						},
						success: res => {
							this.listData = res.data.rows
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			open() {
				// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
				this.$refs.popup.open('top')
			},
			getNewClassList(e) {
				console.log(e.detail.index)
				console.log(this.lawClass)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list?legalExpertiseId=' +
						this.lawClass[e.detail.index].id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.listData = res.data.rows
						console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>
.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}
	
	.image {
		width: 35px;
		height: 35px;
	}
	
	.text {
		font-size: 12px;
		margin-top: 5px;
	}
</style>
