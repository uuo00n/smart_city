<template>
	<view>
		<view class="data">
			<view class="pic">
				<image :src="host+data.imgUrl" mode="" style="width: 100%;"></image>
			</view>
			<view class="title">
				<uni-section :title="data.name" :sub-title="'活动类别:'+data.categoryName" type="circle"></uni-section>
			</view>
			<view class="body" style="padding: 20rpx;">
				{{data.content}}
			</view>
			<view class="footer" style="height: 90rpx;">
				<view>
					<view style="float: left;" class="fpad">报名数：{{data.signupNum}}人</view>
					<view style="float: right;" class="fpad">点赞数：{{data.likeNum}}</view>
				</view>
			</view>
		</view>
		<view class="joinAct" style="padding: 20rpx;">
			<button type="primary" @click="joinAct" v-if="isSignup === false">报名活动</button>
			<button type="primary" disabled="true" v-else>已报名</button>
		</view>
		<view class="comment">
			<view>
				<uni-section title="评论" :sub-title="comment.total+'条'" type="line"></uni-section>
			</view>
			<view class="showComment" style="margin: 0rpx 30rpx;text-align: center;">
				<button size="mini" type="primary" @click="showComment()" v-if="commentSta==false">查看评论</button>
			</view>
			<view class="comment" v-if="commentSta==true">
				<view>
					<uni-list>
						<uni-list-item :title="item.nickName" :note="item.content" :thumb="host+item.avatar"
							v-for="(item,index) in shortData">123</uni-list-item>
					</uni-list>
				</view>
				<view class="comment_list" style="margin: 30rpx;text-align: center;display: flex;">
					<button size="mini" type="primary" @click="showMoreData()"
						v-if=" visibleCount<comment.total">更多评论</button>
					<button size="mini" type="primary" @click="closeComment()">收起评论</button>
				</view>
			</view>
		</view>
		<view class="good_atc">
			<view>
				<uni-section title="活动推荐" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.name" :note="'报名人数:'+item.signupNum+'人'+' '+'点赞数'+item.likeNum+'人'"
						clickable v-for="(item,index) in other" @click="goOuther(item)">
						<image :src="host+item.imgUrl" slot="header"
							style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
		<view class="bottom_text" v-if="commentSta == true">
			<view style="flex: 3;"><input type="text" value="" v-model="inputValue" ref="inputElement"
					placeholder="请输入要提问的内容" @input="onKeyInput" /></view>
			<view style="flex: 1; text-align: center;"><button class="mini-btn" type="primary" size="mini"
					@click="sendCom">发送</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				data: [],
				comment: {
					data: [],
					total: 0
				},
				commentSta: false,
				displayCount: 15,
				visibleCount: 15,
				inputValue: '',
				isSignup: true,
				other: []
			}
		},
		mounted() {
			this.getData()
			this.getComment()
			this.getSign()
			this.getOther()
		},
		methods: {
			getData() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/activity/' + uni.getStorageSync(
						"act_id"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						console.log(uni.getStorageSync("act_id"))
						this.data = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getComment() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/comment/list?activityId=' + uni
						.getStorageSync("act_id"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.comment.total = res.data.total
						this.comment.data = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			showComment() {
				this.commentSta = true
			},
			closeComment() {
				this.commentSta = false
			},
			showMoreData() {
				this.visibleCount += 30;
			},
			onKeyInput: function(event) {
				this.inputValue = event.target.value
			},
			sendCom() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/comment',
					method: 'POST',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {
						"activityId": uni.getStorageSync("act_id"),
						"content": this.inputValue
					},
					success: res => {
						if (res.data.code === 200) {
							this.inputValue = ''
							this.getComment()
							uni.showToast({
								title: '评论成功',
								position: 'bottom',
								icon: 'none'
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getSign() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/signup/check?activityId=' + uni
						.getStorageSync("act_id"),
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.isSignup = res.data.isSignup
					},
					fail: () => {},
					complete: () => {}
				});
			},
			joinAct() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/signup',
					method: 'POST',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {
						"activityId": uni.getStorageSync("act_id")
					},
					success: res => {
						if (res.data.code === 200) {
							uni.showToast({
								title: '报名成功',
								position: 'bottom',
								icon: 'none'
							});
							this.getSign()
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getOther() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/activity/list?recommend=Y&pageNum=1&pageSize=3',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						console.log(res)
						this.other = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goOuther(item) {
				uni.setStorageSync("act_id", item.id)
				uni.redirectTo({
					url: '../activity_about/activity_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		computed: {
			shortData() {
				return this.comment.data.slice(0, this.visibleCount)
			}
		}
	}
</script>

<style>
	.fpad {
		margin: 20rpx;
	}

	.bottom_text {
		width: 100vw;
		padding: 6px 12px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #ededf4;
		position: fixed;
		bottom: 0;
	}
</style>
