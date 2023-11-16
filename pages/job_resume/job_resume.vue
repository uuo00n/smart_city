<template>
	<view>
		<!-- 无简历 -->
		<view v-if="this.userData.length === 0">
			<!-- 新增信息 -->
			<view class="xinzeng">
				<view>
					<uni-section title="基本信息" sub-title="" type="circle"></uni-section>
				</view>
				<view class="input-class">
					用户昵称：
					<view>{{userInfo.userName}}</view>
					邮件：
					<view>{{userInfo.email}}</view>
					电话号码：
					<view>{{userInfo.phonenumber}}</view>
					性别：
					<view v-if="userInfo.sex === '0'">男</view>
					<view v-else>女</view>
				</view>
				<view>
					<uni-section title="求职信息" sub-title="" type="circle"></uni-section>
				</view>
				<view class="input-class" >
					工作经验：
					<input type="text" value="" class="inp" v-model="new_gzjy"/>
					最高学历：
					<input type="text" value="" class="inp" v-model="new_zgxl"/>
					现居住地：
					<input type="text" value="" class="inp" v-model="new_xjzd"/>
					期望职位：
					<input type="text" value="" class="inp" v-model="new_qwzw"/>
					期望薪资：
					<input type="text" value="" class="inp" v-model="new_qwxz"/>
					教育经历：
					<input type="text" value="" class="inp" v-model="new_jyjl"/>
					个人简介：
					<input type="text" value="" class="inp" v-model="new_grjl"/>
					<br>
					<br>
					<br>
				</view>
			</view>
			<view class="btn">
				<button type="primary" class="fixed-button" @click="create">新增简历</button>
			</view>
		</view>
		<!-- 有简历 -->
		<view v-else>
			<!-- 显示修改 -->
			<view class="write" v-if="isEditing === true">
				<view class="xiugai">
					<view>
						<uni-section title="基本信息" sub-title="" type="circle"></uni-section>
					</view>
					<view class="input-class">
						用户昵称：
						<view>{{userInfo.userName}}</view>
						邮件：
						<view>{{userInfo.email}}</view>
						电话号码：
						<view>{{userInfo.phonenumber}}</view>
						性别：
						<view v-if="userInfo.sex === '0'">男</view>
						<view v-else>女</view>
					</view>
					<view>
						<uni-section title="求职信息" sub-title="" type="circle"></uni-section>
					</view>
					<view class="input-class">
						工作经验：
						<input type="text" v-model="userData.experience" class="inp"/>
						最高学历：
						<input type="text" v-model="userData.mostEducation" class="inp"/>
						现居住地：
						<input type="text" v-model="userData.address" class="inp"/>
						期望职位：
						<input type="text" v-model="userData.userName" class="inp"/>
						期望薪资：
						<input type="text" v-model="userData.money" class="inp"/>
						教育经历：
						<input type="text" v-model="userData.education" class="inp"/>
						个人简介：
						<input type="text" v-model="userData.individualResume" class="inp"/>
						<br>
						<br>
						<br>
					</view>
				</view>
			</view>
			<view class="show" v-else>
				<!-- 显示简历 -->
				<view class="xiugai">
					<view>
						<uni-section title="基本信息" sub-title="" type="circle"></uni-section>
					</view>
					<view class="input-class">
						用户昵称：
						<view>{{userInfo.userName}}</view>
						邮件：
						<view>{{userInfo.email}}</view>
						电话号码：
						<view>{{userInfo.phonenumber}}</view>
						性别：
						<view v-if="userInfo.sex === '0'">男</view>
						<view v-else>女</view>
					</view>
					<view>
						<uni-section title="求职信息" sub-title="" type="circle"></uni-section>
					</view>
					<view class="input-class">
						工作经验：
						<view v-if="userData.experience === null">暂无</view>
						<view v-else>{{userData.experience}}</view>
						最高学历：
						<view v-if="userData.mostEducation === null">暂无</view>
						<view v-else>{{userData.mostEducation}}</view>
						现居住地：
						<view v-if="userData.address === null">暂无</view>
						<view v-else>{{userData.address}}</view>
						期望职位：
						<view v-if="userData.userName === null">暂无</view>
						<view v-else>{{userData.userName}}</view>
						期望薪资：
						<view v-if="userData.money === null">暂无</view>
						<view v-else>{{userData.money}}</view>
						教育经历：
						<view v-if="userData.education == null">暂无</view>
						<view v-else>{{userData.education}}</view>
						个人简介：
						<view v-if="userData.individualResume === null">暂无</view>
						<view v-else>{{userData.individualResume}}</view>
						<br>
						<br>
						<br>
					</view>
				</view>
			</view>
			<view class="btn">
				<button type="primary" class="fixed-button" @click="edit">修改简历</button>
			</view>
			<view class="btn" v-if="isEditing">
				<button type="primary" class="fixed-button" @click="save">保存简历</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				token: uni.getStorageSync("token"),
				userData: [],
				userInfo: [],
				isEditing: false
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData() {
				let userId = ''
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/common/user/getInfo',
					method: 'GET',
					header: {
						Authorization: this.token
					},
					data: {},
					success: res => {
						// 测试id
						// this.userId = 2
						// 获取id
						this.userId = res.data.user.userId
						this.userInfo = res.data.user
						// console.log(this.token)
						console.log(this.userInfo)
						// console.log(this.userId)
						uni.request({
							url: 'http://124.93.196.45:10001/prod-api/api/job/resume/queryResumeByUserId/' +
								this.userId,
							method: 'GET',
							data: {},
							header: {
								Authorization: uni.getStorageSync('token')
							},
							success: res => {
								// this.userInfo = res.data
								if (res.data.hasOwnProperty("data")) {
									this.userData = res.data.data
									console.log(this.userData)
								} else {
									console.log(this.userData.length)
									uni.showToast({
										title: '当前暂无个人简历，请新建个人简历',
										position: 'bottom',
										icon: 'none',
										duration: 3000
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
			},
			create(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/job/resume',
					method: 'POST',
					header: {
						Authorization: this.token
					},
					data: {
						"mostEducation": this.new_zgxl,
						"education": this.new_jyjl,
						"address": this.new_xjzd,
						"experience": this.new_gzjy,
						"individualResume": this.new_grjl,
						"money": this.new_qwxz,
						"positionId": this.new_qwzw,
					},
					success: res => {
						if(res.data.code === 200){
							uni.showToast({
								title: '创建简历成功',
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}else{
							uni.showToast({
								title: '创建简历失败',
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			edit() {
				this.isEditing = true
			},
			save() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/job/resume',
					method: 'PUT',
					header: {
						Authorization: this.token
					},
					data: {
						"id":this.userData.id,
						"experience":this.userData.experience,
						"mostEducation":this.userData.mostEducation,
						"address":this.userData.address,
						"userName":this.userData.userName,
						"money":this.userData.money,
						"education":this.userData.education,
						"individualResume":this.userData.individualResume
					},
					success: res => {
						if(res.data.code === 200){
							this.isEditing = false
							uni.showToast({
								title: '修改成功',
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}else{
							uni.showToast({
								title: '保存失败请检查输入的内容',
								position: 'bottom',
								icon: 'none',
								duration: 3000
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
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

	.inp {
		/* background-color: #D3D3D3; */
		margin: 10rpx 10rpx;
		padding: 10rpx;
		border: 1rpx solid;
		border-radius: 15rpx;
	}

	.input-class {
		padding: 0rpx 50rpx;
	}

	.fixed-button {
		position: fixed;
		bottom: 0;
		width: 100%;
		/* 其他样式属性，如背景颜色、边框等 */
	}

	.input-class view {
		margin: 2rpx 20rpx;
		padding: 10rpx;
	}
</style>
