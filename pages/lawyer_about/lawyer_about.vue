<template>
	<view>
		<view class="title">
			<uni-section title="律师介绍" sub-title="" type="line"></uni-section>
		</view>
		<view class="lit-msg">
			<view class="msg-list" style="display: flex;">
				<view style="flex: 1;">
					<image :src="host+lawyerData.avatarUrl" mode="" class="pic"></image>
				</view>
				<view style="flex: 2; padding: 70rpx;">
					<view>姓名：{{lawyerData.name}}</view>
					<view>法律专长：{{lawyerData.legalExpertiseName}}</view>
					<view>咨询人数：{{lawyerData.serviceTimes}}人</view>
					<view>服务次数：{{lawyerData.favorableCount}}次</view>
				</view>
			</view>
		</view>
		<view style="padding: 10rpx;">
			<uni-segmented-control :current="current" :values="ItemClass" @clickItem="changeItem" styleType="text">
			</uni-segmented-control>
			<view style="padding: 10rpx;">
				<view v-show="current === 0">
					<view>律师简介：
						<view class="bd-pad">
							{{lawyerData.baseInfo}}
						</view>
					</view>
					<view>律师教育背景：
						<view class="bd-pad">{{lawyerData.eduInfo}}</view>
					</view>
					<view>执业证编号：
						<view class="bd-pad">
							{{lawyerData.licenseNo}}
						</view>
					</view>
					<view>职业证书：
						<view class="bd-pad">
							<image :src="host+lawyerData.certificateImgUrl" mode=""></image>
						</view>
					</view>
				</view>
				<view v-show="current === 1">
					<uni-list>
						<uni-list-item v-for="(item,index) in secList" style="display: flex;">
							<image slot="header" :src="host+item.fromUserAvatar" mode="" style="" class="header-img"
								style="flex: 1;"></image>
							<view slot="body" style="flex: 4;">
								<view style="padding: 5rpx;">{{item.fromUserName}}</view>
								<view style="padding: 10rpx;">{{item.evaluateContent}}</view>
							</view>
							<view slot="footer" style="flex: 2;">
								<text style="padding: 5rpx;">{{item.createTime.slice(0,10)}}</text>
								<view style="float: right;padding: 10rpx;" @click="like(item)">{{item.likeCount}}
									<uni-icons type="hand-thumbsup" size="20" v-if="item.myLikeState == false">
									</uni-icons>
									<uni-icons type="hand-thumbsup-filled" size="20" v-else></uni-icons>
								</view>
							</view>
						</uni-list-item>
					</uni-list>
				</view>
			</view>
		</view>
		<view class="bottom_text">
			<view style="margin: 10rpx;">
				<button type="primary" size="default" @click="goQs()">免费咨询</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				lawyerData: [],
				ItemClass: ['服务方式', '用户评论'],
				current: 0,
				secList: [],
				likeState: false
			}
		},
		mounted() {
			this.getLawyerMsg()
			this.getSecList()
		},
		methods: {
			getLawyerMsg() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/' + uni
						.getStorageSync("lawyer_msg").id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.lawyerData = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			changeItem(e) {
				if (this.current != e.currentIndex) {
					this.current = e.currentIndex;
				}
			},
			getSecList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list-evaluate?lawyerId=10&pageNum=1&pageSize=100',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {},
					success: res => {
						this.secList = res.data.rows
						// console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			like(item) {
				if (item.myLikeState == false) {
					this.likeState = true
				} else {
					this.likeState = false
				}
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/legal-advice/evaluate-like',
					method: 'POST',
					header: {
						Authorization: uni.getStorageSync("token")
					},
					data: {
						"adviceId": item.adviceId,
						"like": this.likeState
					},
					success: res => {
						if (this.likeState) {
							uni.showToast({
								title: '点赞成功',
								position: "bottom",
								icon: "none"
							});
						} else {
							uni.showToast({
								title: '取消点赞',
								position: "bottom",
								icon: "none"
							});
						}
						this.getSecList()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goQs() {
				uni.navigateTo({
					url: '../lawyer_create/lawyer_create',
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

	.bd-pad {
		padding: 20rpx;
	}

	.header-img {
		height: 80rpx;
		width: 80rpx;
		margin: 10rpx;
	}

	.goods-carts {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		position: fixed;
		left: 0;
		right: 0;
		/* #ifdef H5 */
		left: var(--window-left);
		right: var(--window-right);
		/* #endif */
		bottom: 0;
		margin-bottom: 10rpx;
	}

	.bottom_text {
		width: 100vw;
		justify-content: space-between;
		align-items: center;
		position: fixed;
		bottom: 0;
	}
</style>
