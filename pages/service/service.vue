<template>
	<view style="overflow: hidden; height: 100%">
		<view class="search">
			<view>
				<uni-section title="搜索服务" sub-title="" type="line"></uni-section>
			</view>
			<view class="search-box">
				<uni-search-bar @confirm="search" cancelText="搜索" @cancel="search"></uni-search-bar>
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
					<h4 style="margin: 30rpx;" v-if="inSearch">搜索结果:</h4>
					<view v-if="nr.length ===0" style="text-align: center;" >
						<text>暂无结果</text>
					</view>
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
				search_nr:[],
				qtfl:[],
				inSearch:false,
				sCount:0
			};
		},
		mounted() {
			this.getData();
		},
		methods: {
			search(e) {
				this.sCount += 1
				this.inSearch = true
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/service/list?serviceName='+e.value,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					data: {},
					success: res => {
						this.nr = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
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
				this.inSearch = false
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
				if (sn === '城市地铁') {
					url = '../metro/metro'
				}
				if (sn === '停哪儿') {
					url = '../park/park'
				}
				if (sn === '找工作') {
					url = '../job/job'
				}
				if (sn === '找房子') {
					url = '../house/house'
				}
				if (sn === '智慧交管') {
					url = '../traffic_search/traffic_search'
				}
				if (sn === '宠物医院') {
					url = '../pet_hospital/pet_hospital'
				}
				if (sn === '活动管理') {
					url = '../activity/activity'
				}
				if (sn === '生活缴费') {
					url = '../pay/pay'
				}
				if (sn === '看电影') {
					url = '../movie/movie'
				}
				if (sn === '智慧巴士') {
					url = '../bus/bus'
				}
				if (sn === '政府服务热线') {
					url = '../gov/gov'
				}
				if (sn === '志愿服务') {
					url = '../volunteer/volunteer'
				}
				if (sn === '数字图书馆') {
					url = '../library/library'
				}
				if (sn === '垃圾分类') {
					url = '../waste/waste'
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
