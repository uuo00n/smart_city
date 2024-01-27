<template>
	<view>
		<view>
			<uni-title :title="data.title" align="center" type="h1"></uni-title>
			<view style="text-align: center;">
				<view>{{data.author}}</view>
				<view>{{data.createTime}}</view>
			</view>
		</view>
		<view>
			<view style="text-align: center;">
				<image :src="host+data.imgUrl" mode="aspectFit"></image>
			</view>
			<view class="font" style="padding:10rpx 50rpx;">
				{{data.content}}
			</view>
		</view>
		<view>
			<view>
				<uni-section title="评论" sub-title="" type="circle"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.content" :note="item.createTime" v-for="(item,index) in comment_data"></uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="bottom_text">
			<view style="flex: 3;"><input type="text" value="" v-model="inputValue" ref="inputElement"
					placeholder="说点什么吧" @input="onKeyInput" /></view>
			<view style="flex: 1; text-align: center;"><button class="mini-btn" type="primary" size="mini"
					@click="sendQuestion">发送</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				data: [],
				comment_data: [],
				inputValue: '',
			}
		},
		mounted() {
			this.getNewAbo();
			this.getCom()
		},
		methods: {
			getNewAbo() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news/' + uni
						.getStorageSync("waste_news").id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.data = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getCom() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news-comment/list?newsId=' +
						uni.getStorageSync("waste_news").id + '&pageNum=1&pageSize=10',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.comment_data = res.data.rows
						console.log(this.comment_data)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			sendQuestion() {
				if (this.inputValue != '') {
					uni.request({
						url: 'http://124.93.196.45:10001/prod-api/api/garbage-classification/news-comment',
						method: 'POST',
						header: {
							Authorization: uni.getStorageSync('token')
						},
						data: {
							"newsId": uni.getStorageSync("waste_news").id,
							"content": this.inputValue
						},
						success: res => {
							uni.showToast({
								title: '评论成功',
								position: 'bottom',
								icon: 'none'
							});
							this.getCom()
							this.inputValue = ''
							this.$refs.inputElement.value = ''
						},
						fail: () => {},
						complete: () => {}
					});
				} else {
					uni.showToast({
						title: '评论内容不能为空',
						position: 'bottom',
						icon: 'none'
					});
				}
			},
		}
	}
</script>

<style>
	.bottom_text {
		/* width: 100vw;
		padding: 6px 12px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #ededf4;
		position: absolute; */

		width: 100vw;
		padding: 6px 12px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #ededf4;
		position: fixed;
		bottom: 0;
		/* 将元素定位到页面底部 */
	}
</style>
