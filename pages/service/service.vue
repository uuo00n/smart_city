<template>
	<view style="overflow: hidden; height: 100%">
		<view class="search">
			<view>
				<uni-section title="搜索服务" sub-title="" type="line"></uni-section>
			</view>
			<view class="search-box">
				<uni-search-bar @confirm="search" cancelText="确认" @cancel="search"></uni-search-bar>
			</view>
		</view>
		<view class="fenlei">
			<view class="">
				<uni-section title="全部服务" sub-title="" type="line"></uni-section>
			</view>
			<view class="" style="display: flex">
				<scroll-view scroll-y="" scroll-top="" style="width: 250rpx;border-top: 1rpx #808080 solid;"
					:style="{ height: winHeight }">
					<view v-for="(item, index) in fwlb" :key="index"
						style="padding: 10rpx;text-align: center;border-bottom: 1rpx #808080 solid;"
						@click="getItem(item)">
						{{ item }}
					</view>
				</scroll-view>
				<scroll-view scroll-y :style="{ height: winHeight }"
					style="border-left: 1rpx #808080 solid;border-top: 1rpx #808080 solid;">
					<uni-grid :column="3" :showBorder="false" :highlight="false" :square="false" @change="tapItem">
						<uni-grid-item v-for="(item, index) in nr" :index="index" :key="index">
							<view class="grid-item-box">
								<image :src="baseURL + item.imgUrl" mode="" class="image"></image>
								<text class="text">{{ item.serviceName }}</text>
							</view>
						</uni-grid-item>
					</uni-grid>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				baseURL: 'http://124.93.196.45:10001',
				allData: [],
				fwlb: [],
				nr: [],
				qtfl:[]
			};
		},
		mounted() {
			this.getData();
		},
		methods: {
			search(res) {
				console.log('成功点击确认按钮内容为' + res.value);
			},
			getItem(item) {
				this.showItem(item);
			},
			getData(item) {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/service/list',
					method: 'GET',
					data: {},
					success: (res) => {
						this.allData = res.data.rows;
						const uText = {};
						for (let i = 0; i < this.allData.length; i++) {
							const text = this.allData[i].serviceType;
							if (text === null) {
								continue;
							}
							if (!uText[text]) {
								this.fwlb.push(text);
								uText[text] = true;
							}
						}
						this.fwlb.push('其他');
					},
					fail: () => {},
					complete: () => {}
				});
				this.showItem('便民服务');
			},
			showItem(e) {
				if (e === '其他') {
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/service/list',
						method: 'GET',
						data: {},
						success: (res) => {
							this.qtfl = []
							for (let i = 0; i < this.allData.length; i++) {
								const text = this.allData[i].serviceType;
								if (text === null) {
									this.qtfl.push(this.allData[i]); 
								}
							}
							this.nr = this.qtfl;
						},
						fail: () => {},
						complete: () => {}
					});
				} else {
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/service/list?serviceType=' + e,
						method: 'GET',
						data: {},
						success: (res) => {
							this.nr = res.data.rows;
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapItem(e) {
				let sn = this.nr[e.detail.index].serviceName
				let url = ''
				if (sn === '找工作') {
					url = '../job/job'
				}
				if (sn === '找房子') {
					url = '../house/house'
				}
				if (sn === '智慧交管') {
					url = '../traffic/traffic'
				}
				uni.navigateTo({
					url: url
				});
			}
		},
		computed: {
			winHeight() {
				return uni.getSystemInfoSync().windowHeight + 'px';
			}
		}
	};
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
