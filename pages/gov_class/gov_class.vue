<template>
	<view>
		<view>
			<view>
				<uni-section :title="classData.name+'诉求列表'" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list v-if="classList.length != 0">
					<uni-list-item v-for="(item,index) in classList" clickable @click="goAbout(item)">
						<view slot="body">
							<view>{{item.title}}</view>
							<view style="font-size: 25rpx;">
								<view>承办单位：{{item.undertaker}}</view>
								<view>提交时间：{{item.createTime}}</view>
								<view>处理状态：<text v-if="item.state == 0">未处理</text><text v-else>已处理</text></view>
							</view>
						</view>
						<image :src="host+item.imgUrl" slot="header"
							style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
				<view v-else>
					<uni-title title="暂无诉求" align="center" type="h3"></uni-title>
				</view>
			</view>
		</view>
		<view class="bottom_text">
			<button type="primary" size="default" style="margin: 10rpx;" @click="goCreate">新建诉求</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				classData: [],
				classList: []
			}
		},
		mounted() {
			this.getList()
		},
		methods: {
			getList() {
				this.classData = uni.getStorageSync("gov_class")
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/gov-service-hotline/appeal/my-list?appealCategoryId='+this.classData.id,
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.classList = res.data.rows
						console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goCreate(){
				uni.redirectTo({
					url: '../gov_create/gov_create'
				});
			},
			goAbout(e){
				uni.setStorageSync("gov_about",e)
				uni.navigateTo({
					url: '../gov_about/gov_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
