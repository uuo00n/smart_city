<template>
	<view>
		<view class="job-msg pd">
			<view class="part1" style="margin-top: 30rpx;">
				<view class="mg">
					<h1>{{ data.name }}</h1>
				</view>
				<view style="margin:18rpx 16rpx;">
					<view class="mg">
						<h4>岗位职责:{{ data.obligation }}</h4>
					</view>
					<view class="mg">
						<h4>公司地点:{{ data.address }}</h4>
					</view>
					<view class="mg">
						<h4>薪资待遇:{{ data.salary }}</h4>
					</view>
					<view class="mg">
						<h4>联系人:{{ data.contacts }}</h4>
					</view>
					<view class="mg">
						<h4>职位描述:{{ data.professionName }}</h4>
					</view>
					<view class="mg">
						<h4>职位需求:{{ data.need }}</h4>
					</view>
				</view>
			</view>
			<view class="part2" style="margin-top: 80rpx;">
				<view class="mg">
					<h2>企业介绍</h2>
				</view>
				<view style="margin:18rpx 16rpx;">
					<view>
						<h3>{{company.companyName}}</h3>
					</view>
					<view class="mg">
						<h4>公司简介:{{ company.introductory }}</h4>
					</view>
				</view>
			</view>
		</view>
		<view class="sed-btn">
			<button type="primary" class="btn-abs" @click="resume">投简历</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data: [],
				company: [],
				token: uni.getStorageSync("token")
			};
		},
		mounted() {
			this.getData();
		},
		methods: {
			getData() {
				let id = uni.getStorageSync('job_id');
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/job/post/' + id,
					method: 'GET',
					data: {},
					success: (res) => {
						this.data = res.data.data;
						// console.log(this.data.companyId);
						uni.request({
							url: 'http://124.93.196.45:10001/prod-api/api/job/company/' + this.data
								.companyId,
							method: 'GET',
							data: {},
							success: res => {
								this.company = res.data.data
							},
							fail: () => {},
							complete: () => {}
						});
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// time(){
			// 	let now= new Date(Date.now())
			// 	let year = now.getFullYear();
			// 	let month = now.getMonth()+1;
			// 	let date = now.getDate();
			// 	let hour = now.getHours();
			// 	let min = now.getMinutes();
			// 	let sec = now.getSeconds();
			// 	return `${year}-${month}-${date}-${hour}:${min}:${sec}`;
			// },
			resume() {
				let userId = ''
				uni.setStorageSync("sendJL", this.data)
				console.log(uni.getStorageSync("sendJL"))
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/common/user/getInfo',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.userId = res.data.user.userId
						uni.request({
							url: 'http://124.93.196.45:10001/prod-api/api/job/resume/queryResumeByUserId/' +
								this.userId,
							method: 'GET',
							header: {
								Authorization: uni.getStorageSync('token')
							},
							data: {},
							success: res => {
								if (res.data.hasOwnProperty("data")) {
									uni.request({
										url: 'http://124.93.196.45:10001/prod-api/api/job/deliver',
										method: 'POST',
										header: {
											Authorization: uni.getStorageSync('token')
										},
										data: {
											"companyId": this.data.companyId,
											"postId": this.data.id,
											"postName": this.data.name,
										},
										success: res => {
											if (res.data.code === 200) {
												uni.showToast({
													title: '投递成功',
													position: 'bottom',
													icon: 'none',
													success() {
														setTimeout(() => {
															uni
														.navigateBack(); // 返回上一页  
														}, 3000);
													}
												});

											} else {
												uni.showToast({
													title: '投递失败',
													position: 'bottom',
													icon: 'none'
												});
											}
										},
										fail: () => {},
										complete: () => {}
									});
								} else {
									uni.navigateTo({
										url: '../job_resume/job_resume',
										success: res => {},
										fail: () => {},
										complete: () => {}
									});
								}
							},
							fail: () => {},
							complete: () => {}
						});
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	};
</script>

<style>
	.btn-abs {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		text-align: center;
		line-height: 50px;
	}

	.pd {
		padding: 10rpx;
	}

	.mg {
		margin: 10rpx;
	}
</style>
