<template>
	<view>
		<view class="obj">
			<view class="">
				<uni-section title="搜索结果:" sub-title="" type="circle"></uni-section>
			</view>
			<view class="">
				<uni-list>
					<uni-list-item v-for="(item,index) in adObj">
						<view class="" slot="body" style="text-align: center;">
							<view>分类：{{item.name}}</view>
							<view>
								<image :src="host+item.imgUrl" mode="aspectFit" style="height: 100px;"></image>
							</view>
							<view>描述：{{item.introduce}}</view>
						</view>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="recommend">
			<view class="">
				<uni-section title="该类常见垃圾物品" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="4" :showBorder="false" :highlight="false" :square="false" @change="">
					<uni-grid-item v-for="(item,index) in tsObj" :index="index" :key="index">
						<view class="grid-item-box">
							<image :src="host + item.imgUrl" mode="aspectFit" class="image"></image>
							<text class="text">{{ item.name }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				adObj: [],
				tsObj: []
			}
		},
		mounted() {
			this.Search()

		},
		methods: {
			Search() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-classification/list?name=' +
						uni.getStorageSync("sb"),
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.adObj = res.data.rows
						this.getAbout()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getAbout() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/garbage-example/list?type=' +
						this.adObj[0].id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.tsObj = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			}
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
		height: 60px;
	}

	.text {
		font-size: 12px;
		margin-top: 5px;
	}
</style>
