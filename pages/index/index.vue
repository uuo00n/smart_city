<template>
	<view>
		<view class="search">
			<uni-search-bar radius="5" placeholder="请输入你要搜索的内容" clearButton="auto" cancelButton="none"
				@confirm="search" />
		</view>
		<view class="loop-img">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
				<swiper-item>
					<image src="../../static/loop1.jpeg" mode=""></image>
				</swiper-item>
				<swiper-item>
					<image src="../../static/loop2.jpeg" mode=""></image>
				</swiper-item>
				<swiper-item>
					<image src="../../static/loop3.jpeg" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view>
			<uni-section title="推荐应用" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<uni-grid :column="5" :showBorder="false" :highlight="false" :square="false" @change="tap">
				<uni-grid-item v-for="(item, index) in service.datas" :index="index" :key="index">
					<view class="grid-item-box">
						<image :src="baseURL + item.imgUrl" mode="" class="image"></image>
						<text class="text">{{ item.serviceName }}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view>
			<uni-section title="热门主题" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<uni-grid :column="2" :showBorder="false" :highlight="false" :square="false" @change="">
				<uni-grid-item v-for="(item, index) in zhuti.datas" :index="index" :key="index">
					<view class="grid-item-box titem">
						<image :src="baseURL + item.imgUrl" mode="" class="image"></image>
						<text class="text">{{ item.themeName }}</text>
						<text class="text text-ellipsis">{{ item.createTime }}{{ item.createTime }}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view>
			<uni-section title="新闻" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<view class="uni-padding-wrap uni-common-mt" style="padding:0rpx 20rpx;">
				<uni-segmented-control :values="news.group" @clickItem="getNewsData" />
			</view>
			<view class="news-list" style="padding: 20rpx;">
				<uni-list>
					<uni-list-item title="" note="" v-for="(item,index) in news.newsList_jinri" :key="index"
						:title="item.title" :note="item.createTime.slice(0,10)+' '+'评论数:'+item.commentNum">
						<image :src="'http://124.93.196.45:10001'+item.cover" slot="header"
							style="width: 200rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
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
				baseURL: 'http://124.93.196.45:10001',
				service: {
					datas: []
				},
				zhuti: {
					datas: []
				},
				newDataItem: {
					serviceName: '更多服务',
					imgUrl: '/prod-api/profile/upload/image/2021/05/08/0b4b5757-ac06-4ac5-91eb-dc49e4846959.png',
					pid: '0203'
				},
				news: {
					group: [],
					datas: [],
					newsList_jinri: []
				}
			};
		},
		mounted() {
			this.getToken();
			this.getData();
		},
		methods: {
			getToken() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/login',
					method: 'POST',
					data: {
						username: 'huangjb',
						password: 'huang'
					},
					success: (res) => {
						uni.setStorageSync('token', res.data.token);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			search(res) {
				uni.showToast({
					title: '搜索：' + res.value,
					icon: 'none'
				});
			},
			getData(item) {
				// this.baseURL = localStorage.getItem("baseURL");
				// console.log(this.baseURL)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/service/list',
					method: 'GET',
					data: {},
					timeout: 100000,
					success: (res) => {
						this.service.datas = res.data.rows.slice(0, 9);
						this.service.datas.push(this.newDataItem);
						// console.log(this.service.datas);
					},
					fail: () => {},
					complete: () => {}
				});
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/takeout/theme/list',
					method: 'GET',
					data: {},
					success: (res) => {
						this.zhuti.datas = res.data.data.slice(0, 2);
						// console.log(this.zhuti.datas[1]);
					},
					fail: () => {},
					complete: () => {}
				});
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/press/category/list',
					method: 'GET',
					data: {},
					success: (res) => {
						// console.log(res.data.data);
						this.news.datas = res.data.data
						for (var i = 0; i < res.data.data.length; i++) {
							this.news.group.push(res.data.data[i].name);
						}
						// console.log(this.news.group);
						// console.log(this.news.datas);
					},
					fail: () => {},
					complete: () => {}
				});
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/press/press/list?type=9',
					method: 'GET',
					data: {},
					success: res => {
						this.news.newsList_jinri = res.data.rows;
						// console.log(this.news.newsList_jinri)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getNewsData(e) {
				var newsdataid = this.news.datas[e.currentIndex].id;
				// console.log(newsdataid)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/press/press/list?type=' + newsdataid,
					method: 'GET',
					data: {},
					success: res => {
						this.news.newsList_jinri = res.data.rows;
					},
					fail: () => {},
					complete: () => {}
				});
			},
			tap(item) {
				// console.log(this.service.datas[item.detail.index].serviceName);
				console.log(item.detail.index)
				let url = '';
				if (item.detail.index === 5) {
					url = "../park/park"
				}
				if (item.detail.index === 6) {
					url = "../metro/metro"
				}
				uni.navigateTo({
					url: url
				});
			}
		}
	};
</script>

<style scoped>
	.loop-img image {
		width: 100%;
	}

	.loop-img swiper {
		height: 200px;
	}

	.image {
		width: 35px;
		height: 35px;
	}

	.text {
		font-size: 12px;
		margin-top: 5px;
	}

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

	.titem {
		/* display: flex;
		flex-direction: column;
		align-items: center;
		cursor: pointer; */
		margin: 10px;
		border-radius: 20px;
		/* 设置圆角矩形 */
		/* overflow: hidden; */
		/* 隐藏溢出内容 */
		position: relative;
		padding: 10px;
		background-color: #fff;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		text-align: center;
	}

	.text-ellipsis {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		max-width: 100%;
		/* 设置最大宽度，可根据需要调整 */
	}
</style>
